# soc-anonymizer — SOC 資料去識別化工具

> Author: **Wendy Chang** — Security Engineer (SIEM / ArcSight)

SOC 通報資料（CSV）要丟給 AI 分析，但裡面有 IP、主機名稱、使用者、客戶名稱這些敏感資訊不能外流。
這個工具在**瀏覽器本機**把敏感欄位換成代碼，AI 分析完再一鍵還原回原值。

**資料不離開本機**：純前端單一 HTML 檔，沒有後端、沒有外部 API 呼叫，雙擊就能用，也可以離線使用。

## 流程

```
原始 CSV ──去識別化──▶ 代碼化 CSV ＋ mapping.json（只留在本機）
                         │
                         ▼
                  丟給 AI 分析（工具會自動產生分析 Prompt）
                         │
                         ▼
AI 回覆（CSV / 文字 / Markdown / HTML）──還原──▶ 帶回原始 IP／主機名稱的分析結果
```

## 功能

1. **去識別化**：依欄位類型換成一致的代碼，例如 `[IP_001]`、`[HOST_002]`、`[CLIENT_001]`、`[USER_003]`。同一個值在整份檔案裡都對到同一個代碼，AI 才看得出關聯性
2. **自由文字掃描**：Description / Summary 這類欄位裡夾帶的 IP 也會一起抓出來替換
3. **自動產生 AI Prompt**：去識別化完成後，產生一段可以直接複製貼上的分析 Prompt
4. **還原**：支援上傳 CSV，或直接貼 AI 回覆的文字（Markdown、HTML 也可以），依 mapping.json 換回原值

## 用法

1. 用瀏覽器開啟 `soc-anonymizer.html`
2. 把 CSV 拖進去 → 下載去識別化後的 CSV 跟 `mapping.json`
3. 複製產生的 Prompt，連同 CSV 一起丟給 AI
4. 把 AI 的回覆貼到「還原」頁 → 載入 `mapping.json` → 取得還原後的結果

## 自訂欄位

要處理哪些欄位、用什麼代碼前綴，寫在 HTML 裡的 `SENSITIVE_COLS`（欄位 → 代碼前綴）跟 `TEXT_SCAN_COLS`（要掃自由文字的欄位）。改成你自己的 CSV 欄位名稱就能用在其他格式。

## 安全注意

- `mapping.json` 等於還原鑰匙，**不要**跟去識別化後的資料一起送出去
- 去識別化只處理有設定的欄位跟自由文字裡的 IP，送出前請自己再看一眼

## License

MIT
