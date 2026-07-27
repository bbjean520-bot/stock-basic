---
name: stock-basic
description: >-
  Taiwan finance literacy from CMoney + MoneyDJ wikis (術語、財報、籌碼、技術分析、
  個股／產業百科). Use when the user asks stock basics, 台股名詞, investing 入門,
  company/industry wiki lookup, or needs consistent Chinese finance definitions.
---

# stock-basic（股票基本知識）

雙來源 skill，回答台股／投資常識時使用。

| 來源 | 擅長 | 入口 |
|------|------|------|
| [CMoney 財經百科](https://wiki.cmoney.tw/finance/) | 標準術語定義、入門解說、六大分類索引 | 約 800+ 概念詞 |
| [MoneyDJ 財經百科](https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx) | 超大詞庫、**公司／產業**條目、技術法則、社群更新 | 宣稱約 5 萬+ 詞條 |

## 何時用哪個來源

1. **名詞定義／公式／投資入門** → 先查 [glossary-cmoney.md](glossary-cmoney.md)，再 `WebFetch` 文章。
2. **上市公司、產業、冷門術語、葛蘭碧等寶典** → 開 MoneyDJ 首頁或站內搜尋，再 `WebFetch` 詞條頁（`WikiViewer.aspx?keyid=...`）。
3. **兩邊都有** → 以 CMoney 當清楚定義骨架，MoneyDJ 補實務例子／公司脈絡；衝突時兩邊都標來源，勿混成一家之言。
4. **台股市場骨架**（證交所、三大法人、融資融券等）→ 先讀 [core-taiwan.md](core-taiwan.md)。

## 工作流程

1. 分類問題：定義／公司／產業／技術／總經／稅務。
2. 依上表選來源；需要精確原文時再 fetch，**禁止捏造定義**。
3. 回答附來源連結（CMoney article URL 或 MoneyDJ WikiViewer URL）。
4. 即時股價、下單建議不在本 skill 範圍；行情資料改走 `finmind` 或其他資料 API。

## 回答原則

- 優先台灣市場用語（證交所、櫃買、三大法人、除權息、漲跌停）。
- 先一句定義 → 公式或例子 → 使用限制。
- 信用交易／放空／衍生性商品標明槓桿與風險。
- 非投資顧問意見；不保證報酬。

## 檔案

- [core-taiwan.md](core-taiwan.md) — 台股速查骨架
- [glossary-cmoney.md](glossary-cmoney.md) — CMoney 詞條 → URL 索引
- [sources.md](sources.md) — 兩站差異與查詢方式

## 完成標準

- [ ] 關鍵術語有可追溯來源（已 fetch 或核對索引）
- [ ] 有附連結；台股用語正確
- [ ] 未把美股規則硬套台股；未假裝即時報價
