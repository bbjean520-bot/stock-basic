# 台股官方免費 OpenAPI

三個交易所／櫃買的**開放資料 API**：免註冊、免 API key，HTTP `GET` 回 JSON。  
本檔只做**入口、怎麼選、常用端點與使用限制**；完整欄位與最新清單以各站 Swagger 為準。

**不是即時報價串流，也不是下單 API。** 多數端點是最新一期（或最近交易日）快照。資料與條款屬原站，引用請註明來源。

| 市場 | 官方文件（Swagger UI） | 規格檔 | Base URL | 端點數（撰寫時） |
|------|------------------------|--------|----------|------------------|
| 上市（證交所 TWSE） | https://openapi.twse.com.tw/ | https://openapi.twse.com.tw/swagger.json | `https://openapi.twse.com.tw/v1` | 143 |
| 上櫃／興櫃等（櫃買 TPEx） | https://www.tpex.org.tw/openapi/ | https://www.tpex.org.tw/openapi/swagger.json | `https://www.tpex.org.tw/openapi/v1` | 225 |
| 期貨／選擇權（期交所 TAIFEX） | https://openapi.taifex.com.tw/ | https://openapi.taifex.com.tw/swagger.json | `https://openapi.taifex.com.tw/v1` | 135 |

授權與網站條款（以原頁為準）：

- 證交所：[使用條款](https://www.twse.com.tw/zh/page/terms/use.html)
- 櫃買：[網站使用條款](https://www.tpex.org.tw/web/inc/gtsm_disclaimer.php?l=zh-tw)
- 期交所：[網站使用條款](https://www.taifex.com.tw/cht/edu/userTerms)
- 政府資料開放授權：[data.gov.tw/license](https://data.gov.tw/license)

---

## 何時用哪一套

| 你要的資料 | 用 |
|------------|----|
| 上市股票行情、本益比／殖利率、融資融券、除權息、上市月營收／財報、上市基本資料、加權指數 | **TWSE** |
| 上櫃／興櫃行情、上櫃三大法人、上櫃月營收／財報、權證、債券、黃金現貨、開放式基金 | **TPEx** |
| 期貨／選擇權日行情、Put/Call、期貨三大法人、大額交易人未平倉、保證金、每筆成交 | **TAIFEX** |
| 名詞「是什麼、怎麼算」 | 不要用 API；回 [glossary-cmoney.md](glossary-cmoney.md) / [core-taiwan.md](core-taiwan.md) |

上市與上櫃是**兩套獨立 API**，同一類資料欄位名常不一致（上市多中文 key，上櫃常見英文 key）。要做全市場必須自己對映。

---

## 共通規則

1. **全部 `GET`、多數無查詢參數。** 路徑本身就是資料集；加上 `?date=` 通常不會變成歷史查詢。
2. **快照不是歷史庫。** 多數回「最新一期／最近交易日」全市場陣列。要歷史就自己定期抓下來存。
3. **回傳幾乎都是 JSON 陣列**，欄位值常是**字串**（數字也是字串），空值可能是全形空白或「－」。
4. **日期格式不統一。** TWSE／TPEx 常見民國年（`1150904`＝2026-09-04）；TAIFEX 常見西元（`20260904`）。先看回傳再轉。
5. **官方未在回應裡標明確速率限制。** 請節流、快取、失敗重試；不要高頻輪詢當即時行情。條款也禁止用爬蟲去抓**非 OpenAPI** 的官網頁。
6. **櫃買部分路徑會 302 回官網首頁。** 先在 [Swagger](https://www.tpex.org.tw/openapi/) 用 Try it out 確認；請求可加上與官方 UI 相同的快取標頭：`If-Modified-Since: Mon, 26 Jul 1997 05:00:00 GMT`、`Cache-Control: no-cache`、`Pragma: no-cache`。
7. **端點會增刪改名。** 本檔是索引；以各站 `swagger.json` 為準。

最小呼叫（PowerShell）：

```powershell
Invoke-RestMethod https://openapi.twse.com.tw/v1/holidaySchedule/holidaySchedule
```

```powershell
Invoke-RestMethod https://openapi.taifex.com.tw/v1/PutCallRatio
```

```powershell
$h = @{
  "If-Modified-Since" = "Mon, 26 Jul 1997 05:00:00 GMT"
  "Cache-Control" = "no-cache"
  "Pragma" = "no-cache"
}
Invoke-RestMethod https://www.tpex.org.tw/openapi/v1/tpex_mainboard_quotes -Headers $h
```

---

## 證交所 TWSE（上市）

文件：https://openapi.twse.com.tw/  
分類：公司治理、證券交易、財務報表、指數、權證、券商資料、其他。

### 常用端點

完整 URL = `https://openapi.twse.com.tw/v1` + 路徑。

| 用途 | 路徑 |
|------|------|
| 上市個股日成交（開高低收、量值） | `/exchangeReport/STOCK_DAY_ALL` |
| 日收盤價與月平均價 | `/exchangeReport/STOCK_DAY_AVG_ALL` |
| 本益比、殖利率、股價淨值比 | `/exchangeReport/BWIBBU_ALL` |
| 大盤統計 | `/exchangeReport/MI_INDEX` |
| 集中市場每日成交 | `/exchangeReport/FMTQIK` |
| 融資融券餘額 | `/exchangeReport/MI_MARGN` |
| 除權除息預告 | `/exchangeReport/TWT48U_ALL` |
| 開（休）市日期 | `/holidaySchedule/holidaySchedule` |
| 加權指數歷史 | `/indicesReport/MI_5MINS_HIST` |
| 上市公司基本資料 | `/opendata/t187ap03_L` |
| 上市每日重大訊息 | `/opendata/t187ap04_L` |
| 上市每月營業收入 | `/opendata/t187ap05_L` |
| 上市股利分派 | `/opendata/t187ap45_L` |
| 綜合損益表（一般業） | `/opendata/t187ap06_L_ci` |
| 資產負債表（一般業） | `/opendata/t187ap07_L_ci` |
| 外資／陸資持股前 20 名 | `/fund/MI_QFIIS_sort_20` |
| 當沖標的與統計 | `/exchangeReport/TWTB4U` |
| 注意股票 | `/announcement/notice` |

財報另有金融業、金控、保險、證券期貨、礦業等產業格式（路徑尾碼 `_basi`、`_fh`、`_ins`、`_bd`、`_mim`），以及公開發行公司（`_P`／`_X`）版本，見 Swagger「財務報表」。

**缺口：** 這套 OpenAPI **沒有**常見的「上市個股三大法人買賣超」日報（網站上的 T86 類報表）。上櫃三大法人在 TPEx；期貨三大法人在 TAIFEX。上市現貨三大法人請改查證交所官網報表或其他已授權資料源，不要假裝 OpenAPI 有這張表。

---

## 櫃買 TPEx（上櫃／興櫃等）

文件：https://www.tpex.org.tw/openapi/  
分類（Swagger tag）：上櫃、指數系列、公司治理、債券、興櫃、權證、開放式基金、黃金現貨、創櫃、券商資料、財務報表。

完整 URL = `https://www.tpex.org.tw/openapi/v1` + 路徑。

| 用途 | 路徑 |
|------|------|
| 上櫃收盤行情 | `/tpex_mainboard_quotes` |
| 上櫃本益比／殖利率／股淨比 | `/tpex_mainboard_peratio_analysis` |
| 上櫃融資融券餘額 | `/tpex_mainboard_margin_balance` |
| 上櫃三大法人買賣明細 | `/tpex_3insti_daily_trading` |
| 上櫃三大法人買賣金額彙總 | `/tpex_3insti_summary` |
| 上櫃外資／陸資買賣超彙總 | `/tpex_3insti_qfii_trading` |
| 櫃買指數歷史 | `/tpex_index` |
| 上櫃公司基本資料 | `/mopsfin_t187ap03_O` |
| 興櫃公司基本資料 | `/mopsfin_t187ap03_R` |
| 上櫃每日重大訊息 | `/mopsfin_t187ap04_O` |
| 上櫃每月營業收入 | `/mopsfin_t187ap05_O` |
| 興櫃每月營業收入 | `/t187ap05_R` |
| 上櫃綜合損益表（一般業） | `/mopsfin_t187ap06_O_ci` |
| 上櫃資產負債表（一般業） | `/mopsfin_t187ap07_O_ci` |

權證、債券、黃金現貨、創櫃、開放式基金見 Swagger 對應分類。部分舊名稱（例如部分「highlight」路徑）可能 302，以現行 Swagger 為準。

---

## 期交所 TAIFEX（期貨／選擇權）

文件：https://openapi.taifex.com.tw/  
分類在 Swagger 標成「資料查詢API」（約 135 支）。完整 URL = `https://openapi.taifex.com.tw/v1` + 路徑。

| 用途 | 路徑 |
|------|------|
| 期貨每日行情 | `/DailyMarketReportFut` |
| 選擇權每日行情 | `/DailyMarketReportOpt` |
| 臺指選擇權 Put/Call 比 | `/PutCallRatio` |
| 選擇權每日 Delta | `/DailyOptionsDelta` |
| 三大法人總表（依日期） | `/MarketDataOfMajorInstitutionalTradersGeneralBytheDate` |
| 三大法人（各期貨契約、依日期） | `/MarketDataOfMajorInstitutionalTradersDetailsOfFuturesContractsBytheDate` |
| 期貨大額交易人未沖銷 | `/OpenInterestOfLargeTradersFutures` |
| 選擇權大額交易人未沖銷 | `/OpenInterestOfLargeTradersOptions` |
| 最後結算價 | `/FinalSettlementPrice` |
| 指數類保證金 | `/IndexFuturesAndOptionsMargining` |
| 每日期貨每筆成交 | `/TimeAndSalesData` |
| 每日選擇權每筆成交 | `/OptionsTimeAndSalesData` |

期貨／選擇權有槓桿與追繳風險；解釋部位、保證金、未平倉時要講清楚，且**不是投資建議**。

歷史日 K 若 OpenAPI 只有最近交易日，期交所官網另有下載區；那是網站下載，不是本 OpenAPI 的查詢參數。

---

## 給 AI／程式的用法

1. 先判斷市場（上市／上櫃／期貨），再選上表端點；沒列出的先開該站 Swagger 搜 summary。
2. 需要精確欄位時再 `GET` 該 URL 或抓 `swagger.json`，**不要捏造欄位名**。
3. 回答時附**實際呼叫的官方 URL**；寫日期要標民國或西元。
4. 不要把快照講成「任意歷史區間查詢」，也不要講成即時 tick。
5. 百科定義仍走 CMoney／MoneyDJ；官方 API 只負責**數字與名單**。

對照：[core-taiwan.md](core-taiwan.md)（市場骨架）、[sources.md](sources.md)（百科怎麼查）。
