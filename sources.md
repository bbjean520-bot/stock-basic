# CMoney vs MoneyDJ：差異速覽

| 面向 | CMoney 財經百科 | MoneyDJ 財經百科 |
|------|-----------------|------------------|
| 網址 | https://wiki.cmoney.tw/finance/ | https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx |
| 定位 | 教學型「概念百科」 | 理財站「大詞庫／知識庫」 |
| 規模 | 數百～約千級概念詞（本 skill 索引約 819） | 站上標示約 **53000+** 詞條 |
| 內容重心 | 投資入門、市場、財報、總經、稅務、風險等**標準名詞** | 名詞 + **大量公司／產業頁**、技術法則、持續編修 |
| 分類 | 六大固定分類（入門／股市／財報／總經／保險稅務／風險） | 股票市場、金融商品、理財規劃、投資寶典等，另有熱門／最近更新 |
| URL 型態 | 可讀 slug：`/finance/article/price-earnings-ratio` | GUID：`WikiViewer.aspx?keyid=...` |
| 寫作風格 | 官方編輯審核、摘要＋FAQ 教學文 | 多編者、人氣／更新日期、偏條目庫 |
| 最適合 | 「這個詞是什麼、怎麼算」 | 「這家公司／這個冷門詞／技術寶典」 |
| 本 skill 用法 | 主索引見 `glossary-cmoney.md` | 用首頁搜尋或熱門／最近更新定位後再 fetch |

## 查 MoneyDJ 的實務方式

1. 開 https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx  
2. 用頁面「搜尋條目」找詞；或從熱門／最近更新點進。  
3. 詞條 URL 形如：`https://www.moneydj.com/KMDJ/Wiki/WikiViewer.aspx?keyid=<uuid>`  
4. `WebFetch` 該頁後再摘要；保留 keyid 連結給使用者。

## 官方免費 OpenAPI（數字，不是百科）

名詞解釋仍走上面兩站。要**官方開放資料**時改讀 [openapi-tw.md](openapi-tw.md)：

| 站 | Swagger UI |
|----|------------|
| 臺灣證券交易所（上市） | https://openapi.twse.com.tw/ |
| 證券櫃檯買賣中心（上櫃／興櫃等） | https://www.tpex.org.tw/openapi/ |
| 臺灣期貨交易所（期貨／選擇權） | https://openapi.taifex.com.tw/ |

免註冊、免金鑰；多數是最新一期快照，不是任意區間歷史庫，也**不是即時串流**（`openapi.twse.com.tw` 沒有盤中價）。

## 盤中最新價（不是百科、也不是 OpenAPI）

偶爾查現價讀 [quotes-realtime.md](quotes-realtime.md)：

| 來源 | 說明 |
|------|------|
| 證交所 MIS `getStockInfo.jsp` | 網站內部 JSON，免 key；**每 5–10 分鐘最多一次** |
| Yahoo Finance chart | 備援；`regularMarketPrice` 約延遲 15–20 分鐘 |
| 券商 API／付費源 | 高頻、逐筆、長期合法的做法 |

## 授權與引用

百科與官方資料皆為原站著作；本 skill 只做**索引與查閱流程**，回答時應附原連、勿整篇轉貼當自有著作上傳散佈。 OpenAPI 須遵守各交易所使用條款與[政府資料開放授權](https://data.gov.tw/license)。MIS 是網站內部端點、非公開 API，須低頻使用，見 [quotes-realtime.md](quotes-realtime.md) 與[證交所使用條款](https://www.twse.com.tw/zh/page/terms/use.html)。
