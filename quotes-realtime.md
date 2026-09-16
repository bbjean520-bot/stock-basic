# 台股盤中最新價（非官方 OpenAPI）

證交所官方 OpenAPI（[`openapi.twse.com.tw`](https://openapi.twse.com.tw/)）**只有盤後／最新一期快照，沒有即時行情。** 不要再到那裡找盤中價。

本檔三種來源，由鬆到嚴：

| 用途 | 來源 | 延遲／限制 |
|------|------|------------|
| 偶爾查一兩檔最新成交 | 證交所 MIS 網站內部 JSON | 看起來接近即時，但**不是公開 API**；須低頻 |
| MIS 失敗時的備援 | Yahoo Finance chart | 約延遲 15–20 分鐘 |
| 長期穩定、可高頻、要合法 | 券商 API 或付費資訊源 | 需開戶／申請／付費 |

**不是 tick 串流，也不是下單。** 內容僅供學習，不構成投資建議。

---

## 何時用哪個

| 你要的 | 用 |
|--------|----|
| 收盤、月營收、財報、休市日、三大法人（開放資料） | [openapi-tw.md](openapi-tw.md)，不要用本檔 |
| 「2330 現在多少錢」（偶爾、低頻） | 下面 MIS |
| MIS 擋下來／逾時 | Yahoo Finance |
| 盤中每分鐘、逐筆、買一賣一、推播機器人 | **不要打 MIS**；改券商 API 或付費源 |
| 名詞「最新價是什麼」 | 百科，不要打 API |

---

## 證交所 MIS（網站內部端點）

免 API key、免註冊。這是 [MIS 看盤頁](https://mis.twse.com.tw/stock/index.jsp) 自己打的 JSON，**不是** `openapi.twse.com.tw` 的公開 API。

[證交所使用條款](https://www.twse.com.tw/zh/page/terms/use.html)禁止自動化大量抓取官網。請：

- **每 5–10 分鐘最多一次**（同一檔或同一批都算）；不要 while-loop、不要每秒、不要每分鐘輪詢
- 查完快取結果；同一輪對話不要重複打同一檔
- 不要一次串上百檔、不要當全市場掃描器
- 失敗就說查不到，改 Yahoo 或停；不要狂重試

### 請求

```
GET https://mis.twse.com.tw/stock/api/getStockInfo.jsp?ex_ch=tse_2330.tw&json=1&delay=0
```

建議標頭：

| Header | 值 |
|--------|-----|
| `User-Agent` | `Mozilla/5.0`（或一般瀏覽器 UA） |
| `Referer` | `https://mis.twse.com.tw/stock/index.jsp` |

### `ex_ch` 格式

| 市場 | 寫法 | 例 |
|------|------|-----|
| 上市 | `tse_代號.tw` | `tse_2330.tw` |
| 上櫃 | `otc_代號.tw` | `otc_6488.tw` |

多檔用 `|` 串接，例如：`tse_2330.tw|otc_6488.tw`

代號四碼即可；不要寫成 `2330.TW`（那是 Yahoo 格式）。

### 回傳欄位（`msgArray[]`）

JSON 裡每檔一筆，在 `msgArray`。常用鍵：

| 鍵 | 意義 |
|----|------|
| `c` | 代號 |
| `n` | 名稱 |
| `z` | 最新成交價 |
| `y` | 昨收 |
| `o` / `h` / `l` | 開／高／低 |
| `v` | 累積成交量 |
| `tv` | 當盤成交量 |
| `t` | 時間 |
| `d` | 日期 |
| `ex` | 上市別 |
| `pz` | 前次成交價 |

**`z` 若為 `"-"`**（字串減號）＝這一盤還沒成交 → 最新價改用 `pz`；`pz` 也沒有再用 `y`。回答時要講清楚是「尚無成交、改用前次／昨收」，不要把 `-` 講成股價。

其他鍵可能出現在同一物件裡；**沒列在上表就不要猜意思**。`msgArray` 空、缺檔、或 HTTP 失敗 → 說查不到，不准用記憶股價補。

### 最小呼叫（PowerShell）

```powershell
$h = @{
  "User-Agent" = "Mozilla/5.0"
  "Referer"    = "https://mis.twse.com.tw/stock/index.jsp"
}
Invoke-RestMethod "https://mis.twse.com.tw/stock/api/getStockInfo.jsp?ex_ch=tse_2330.tw&json=1&delay=0" -Headers $h
```

上市＋上櫃各一檔：

```powershell
Invoke-RestMethod "https://mis.twse.com.tw/stock/api/getStockInfo.jsp?ex_ch=tse_2330.tw|otc_6488.tw&json=1&delay=0" -Headers $h
```

回答時附**實際呼叫的 URL**（可把 `|` 寫進連結或程式碼區塊）。標明資料時間 `d`＋`t`。

---

## 備援：Yahoo Finance

MIS 失敗、被擋、或你只需要大約數字時：

```
GET https://query1.finance.yahoo.com/v8/finance/chart/2330.TW?interval=1m&range=1d
```

讀 `chart.result[0].meta.regularMarketPrice`。這通常**延遲約 15–20 分鐘**，不要講成即時。

| 市場 | Yahoo 代號 |
|------|------------|
| 上市 | `2330.TW` |
| 上櫃 | `6488.TWO` |

不要跟 MIS 的 `tse_2330.tw` 混用。

```powershell
Invoke-RestMethod "https://query1.finance.yahoo.com/v8/finance/chart/2330.TW?interval=1m&range=1d"
```

---

## 長期穩定合法：券商或付費源

要盤中高頻、逐筆、五檔、自動通知，請走**已授權**管道，例如券商 API（群益、元大、富邦、統一等，需申請開通）或付費行情。本包不提供繞過條款的爬蟲。

下單也不在本 skill 範圍。

---

## 給 AI／程式的用法

1. 使用者問「現價／盤中價」→ 讀本檔；問收盤／財報 → [openapi-tw.md](openapi-tw.md)。
2. 先 MIS（低頻）→ 失敗再用 Yahoo → 還是沒有就說查不到。
3. 實際 `GET` 之後才能報數字；禁止用訓練記憶裡的股價。
4. 附來源 URL 與時間；MIS 要註明「網站內部端點、非官方公開 API」。
5. 訊號牆的 `tick`／每分鐘 `intraday` **不要**打 MIS；那些要券商或付費源。MIS 只適合偶爾查價或低頻刷新。

對照：[openapi-tw.md](openapi-tw.md)（盤後開放資料）、[core-taiwan.md](core-taiwan.md)。
