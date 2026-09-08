# stock-basic

台股／投資**基本常識知識包**，整理自：

- [CMoney 財經百科](https://wiki.cmoney.tw/finance/)：術語定義、入門解說較清楚
- [MoneyDJ 財經百科](https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx)：詞庫大、公司／產業條目多
- 官方免費 OpenAPI：[證交所](https://openapi.twse.com.tw/)、[櫃買](https://www.tpex.org.tw/openapi/)、[期交所](https://openapi.taifex.com.tw/)（免金鑰，見 `openapi-tw.md`）

給人查、也給各種 AI 助手當參考資料用。**不是投資建議**，也不是即時行情串流。

---

## 怎麼裝（跟 AI 講，不要自己下載）

本知識包給課程兩套 AI 用：**Claude Code**、**Hermes**。學生把下面整段貼給正在用的那一套即可。

```text
請把 https://github.com/duang0615/stock-basic 裝成我的個人 skill，名稱 stock-basic。
你自己執行，不要叫我下載 ZIP 或手動複製檔案。

- 你是 Claude Code：裝到 ~/.claude/skills/stock-basic/
- 你是 Hermes：裝到 ~/.hermes/skills/stock-basic/
- Windows：把 ~ 換成 %USERPROFILE%

資料夾不存在就 git clone --depth 1 https://github.com/duang0615/stock-basic.git 到上面的路徑。
已存在就進該資料夾 git pull。
必須整包 clone，不要只抓 SKILL.md（還要 core-taiwan.md、glossary-cmoney.md、sources.md、openapi-tw.md、signals-wall.md、signals-wall.yaml）。
裝完確認 SKILL.md、core-taiwan.md、glossary-cmoney.md、sources.md、openapi-tw.md、signals-wall.md、signals-wall.yaml 都在該資料夾根目錄，然後告訴我成功。
之後問台股請遵守這個 skill：定義走百科，官方數字走證交所／櫃買／期交所 OpenAPI；要做訊號牆就讀 signals-wall.md／yaml。附來源，非投資建議。
```

裝好後開新對話，或說「用 stock-basic」。要更新再說「把 stock-basic 從 GitHub pull 一次」。

| 檔案 | 用途 |
|------|------|
| `SKILL.md` | 給 AI 的使用規則（何時查哪個來源、怎麼回答） |
| `core-taiwan.md` | 台股骨架速查（市場、籌碼、財報、技術面） |
| `glossary-cmoney.md` | CMoney 詞條索引＋文章連結 |
| `sources.md` | 兩站差異與 MoneyDJ 查法 |
| `openapi-tw.md` | 證交所／櫃買／期交所免費 OpenAPI 入口與常用端點 |
| `signals-wall.md` | 常見多空策略：給其他 AI 架「訊號牆」的說明 |
| `signals-wall.yaml` | 訊號牆機器規格（名稱、多空、觸發條件、資料欄位） |

---

## 裝好之後怎麼問

直接問「什麼是本益比」「現股當沖是什麼」「怎麼用官方 API 抓上市月營收」。  
要更新 skill 時跟 AI 說：把 stock-basic 從 GitHub 再 pull 一次。

---

## 怎麼選（簡記）

| 你想問的 | 優先 |
|----------|------|
| 名詞定義、公式、投資入門 | CMoney（索引在 `glossary-cmoney.md`） |
| 上市公司、產業、冷門術語、技術寶典 | MoneyDJ（見 `sources.md`） |
| 兩邊都有 | 定義以 CMoney 為主，MoneyDJ 補例子；衝突就兩邊都標來源 |
| 官方行情／財報／籌碼／期貨開放資料 | 證交所／櫃買／期交所 OpenAPI（見 `openapi-tw.md`） |
| 常見多空訊號、訊號牆、通知我 | `signals-wall.md`＋`signals-wall.yaml`（可計算規則，不是明牌） |

細節規則寫在 `SKILL.md`。

---

## 注意

- 百科原文版權屬原站；本 repo 只做索引、摘要流程與查閱指引，請點回原站看全文。
- 不含即時 tick／下單功能。官方快照走 `openapi-tw.md`；更長歷史或封裝 API 可另接券商、FinMind 等。
- 內容僅供學習，**不構成任何投資建議**。
