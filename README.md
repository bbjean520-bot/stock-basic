# stock-basic

台股／投資**基本常識知識包**，整理自：

- [CMoney 財經百科](https://wiki.cmoney.tw/finance/)：術語定義、入門解說較清楚
- [MoneyDJ 財經百科](https://www.moneydj.com/KMDJ/Wiki/WikiHome.aspx)：詞庫大、公司／產業條目多

給人查、也給各種 AI 助手當參考資料用。**不是投資建議**，也不是即時行情。

---

## 怎麼拿到檔案（大家都能用）

任選一種：

1. **下載 ZIP**（最簡單）  
   打開 repo 頁 → 綠色 **Code** → **Download ZIP** → 解壓縮。

2. **用 Git 複製**

```bash
git clone https://github.com/duang0615/stock-basic.git
```

解壓或 clone 後，記得裡面有這些檔：

| 檔案 | 用途 |
|------|------|
| `SKILL.md` | 給 AI 的使用規則（何時查哪個來源、怎麼回答） |
| `core-taiwan.md` | 台股骨架速查（市場、籌碼、財報、技術面） |
| `glossary-cmoney.md` | CMoney 詞條索引＋文章連結 |
| `sources.md` | 兩站差異與 MoneyDJ 查法 |

---

## 怎麼給 AI 用（依你手上的工具）

下面挑你有的做即可，**不必裝 Cursor**。

### A. ChatGPT / Claude 網頁（最通用）

1. 新建一個專案／自訂 GPT／Project（名稱可叫「台股常識」）。
2. 把資料夾裡的 Markdown **全部上傳**當知識庫（至少要有 `SKILL.md`、`core-taiwan.md`、`glossary-cmoney.md`、`sources.md`）。
3. 在系統提示或專案說明貼一句：

```text
回答台股／投資名詞時，請遵守上傳的 SKILL.md：先查 glossary-cmoney.md 或 core-taiwan.md，
需要細節再請我提供百科連結；不要捏造定義；附上來源連結；非投資建議。
```

之後直接問「什麼是本益比」「三大法人是什麼」即可。

### B. Claude Code / 終端機 AI

把本 repo clone 到專案旁，開對話時指定：

```text
請先讀 stock-basic/SKILL.md 與 core-taiwan.md，之後用這套規則回答台股問題。
```

或把整個 `stock-basic` 資料夾加進工作區，讓 AI 能讀這些檔。

### C. Cursor（可選）

若你用 Cursor Agent Skills，把整個資料夾放到：

- Windows：`C:\Users\<你的帳號>\.cursor\skills\stock-basic\`
- macOS / Linux：`~/.cursor/skills/stock-basic/`

重開 Agent 對話，或說「用 stock-basic」。  
也可用 Cursor 的 skill／規則匯入功能指向本 repo 的 `SKILL.md`。

### D. 其他編輯器（VS Code Copilot、Windsurf、Continue 等）

把 `stock-basic` 放進工作區，或在規則／system prompt 寫：

```text
台股名詞請依 ./stock-basic/SKILL.md 與 glossary-cmoney.md 回答，並附來源連結。
```

### E. 自己查（不用 AI）

- 先看 `core-taiwan.md` 建立框架  
- 再在 `glossary-cmoney.md` 用搜尋找詞條，點連結到 CMoney 原文  
- 公司／產業／冷門詞看 `sources.md` 的 MoneyDJ 查法  

---

## 兩站怎麼選（簡記）

| 你想問的 | 優先 |
|----------|------|
| 名詞定義、公式、投資入門 | CMoney（索引在 `glossary-cmoney.md`） |
| 上市公司、產業、冷門術語、技術寶典 | MoneyDJ（見 `sources.md`） |
| 兩邊都有 | 定義以 CMoney 為主，MoneyDJ 補例子；衝突就兩邊都標來源 |

細節規則寫在 `SKILL.md`。

---

## 注意

- 百科原文版權屬原站；本 repo 只做索引、摘要流程與查閱指引，請點回原站看全文。
- 不含即時股價／下單功能；行情請另接資料來源（券商 API、FinMind 等）。
- 內容僅供學習，**不構成任何投資建議**。
