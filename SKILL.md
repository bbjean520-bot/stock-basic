---
name: stock-basic
description: >-
  Taiwan finance literacy from CMoney + MoneyDJ wikis (術語、財報、籌碼、技術分析、
  個股／產業百科), plus official free TWSE / TPEx / TAIFEX OpenAPI indexes.
  Use when the user asks stock basics, 台股名詞, investing 入門,
  company/industry wiki lookup, official open data APIs, needs
  consistent Chinese finance definitions, or wants a 訊號牆 /
  常見多空策略 / 趨勢向上訊號 / 趨勢向下訊號 / 通知我.
---

# stock-basic（股票基本知識）

百科雙來源＋官方開放資料，回答台股／投資常識與官方數字時使用。

| 來源 | 擅長 | 入口 |
|------|------|------|
| [CMoney 財經百科](https://wiki.cmoney.tw/finance/) | 標準術語定義、入門解說、六大分類索引 | 約 800+ 概念詞 |
| [MoneyDJ 財經百科](https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx) | 超大詞庫、**公司／產業**條目、技術法則、社群更新 | 宣稱約 5 萬+ 詞條 |
| 官方 OpenAPI（證交所／櫃買／期交所） | 免費、免金鑰的開放資料（行情快照、財報、籌碼、期貨） | 見 [openapi-tw.md](openapi-tw.md) |

## 何時用哪個來源

1. **名詞定義／公式／投資入門** → 先查 [glossary-cmoney.md](glossary-cmoney.md)，再 `WebFetch` 文章。
2. **上市公司、產業、冷門術語、葛蘭碧等寶典** → 開 MoneyDJ 首頁或站內搜尋，再 `WebFetch` 詞條頁（`WikiViewer.aspx?keyid=...`）。
3. **兩邊都有** → 以 CMoney 當清楚定義骨架，MoneyDJ 補實務例子／公司脈絡；衝突時兩邊都標來源，勿混成一家之言。
4. **台股市場骨架**（證交所、三大法人、融資融券等）→ 先讀 [core-taiwan.md](core-taiwan.md)。
5. **要官方數字／名單**（收盤行情、月營收、休市日、上櫃三大法人、期貨 Put/Call 等）→ 先讀 [openapi-tw.md](openapi-tw.md)，再 `GET` 對應端點；完整清單以各站 Swagger 為準。
6. **訊號牆／常見多空策略／通知我** → 先讀 [signals-wall.md](signals-wall.md) 與 [signals-wall.yaml](signals-wall.yaml)。每一則先讀 `when`（人話條件，例如「突破月線」＝昨天收在 20 日均線下、今天收盤站上去），再實作 `fire`。不要只看名稱自己猜。不要改中文名稱、不要發明小虎隊成分股、不要保證獲利或自動下單。

## 工作流程

1. 分類問題：定義／公司／產業／技術／總經／稅務／**官方資料**／**訊號牆**。
2. 依上表選來源；需要精確原文或欄位時再 fetch，**禁止捏造定義或欄位名**。
3. 回答附來源連結（CMoney article URL、MoneyDJ WikiViewer URL，或實際呼叫的 OpenAPI URL）。
4. 即時 tick、下單不在本 skill 範圍。歷史或官方快照優先走 [openapi-tw.md](openapi-tw.md) 的免費 API；需要更長歷史或便利封裝時再輔以 FinMind 等第三方。訊號牆只提供規則；真正盤中推播要接券商或行情來源。
5. 使用者要求時可以做相關分析，但數字與結論必須來自本步驟實際查到的資料。沒查到就停，不要用庫外資料或猜測補完。

## 回答原則

- 優先台灣市場用語（證交所、櫃買、三大法人、除權息、漲跌停）。
- 先一句定義 → 公式或例子 → 使用限制。
- 信用交易／放空／衍生性商品標明槓桿與風險。
- 非投資顧問意見；不保證報酬。
- **分析可做、猜測不可做。** 依使用者要求可以分析，但必須用本包指定資料庫實際查出的數據（官方 OpenAPI、已 fetch 的百科頁、signals-wall 規則搭配這些數據）。禁止用訓練記憶、沒查過的來源、或庫外數字當答案。查不到就明說查不到，不准猜。

## 檔案

- [core-taiwan.md](core-taiwan.md) — 台股速查骨架
- [glossary-cmoney.md](glossary-cmoney.md) — CMoney 詞條 → URL 索引
- [sources.md](sources.md) — 兩站差異與查詢方式
- [openapi-tw.md](openapi-tw.md) — 證交所／櫃買／期交所免費 OpenAPI
- [signals-wall.md](signals-wall.md) — 常見多空策略：給其他 AI 架訊號牆
- [signals-wall.yaml](signals-wall.yaml) — 訊號牆機器規格

## 完成標準

- [ ] 關鍵術語有可追溯來源（已 fetch 或核對索引）
- [ ] 有附連結；台股用語正確
- [ ] 未把美股規則硬套台股；未假裝即時報價
- [ ] 分析／數字來自實際查詢的指定資料庫；沒有用庫外答案或猜測補洞
