# AGENTS.md — UbuntuWebCLI AI 工作指引

**開始任何任務前，先讀 [rule.md](rule.md)、[plan/PROJECT_PLAN.md](plan/PROJECT_PLAN.md) 與 [plan/ACCEPTANCE_MATRIX.md](plan/ACCEPTANCE_MATRIX.md)。** 本檔為 AI 工具指引，不能凌駕 `rule.md` 或使用者最新明確指示。

## 任務分類與權限
| 類別 | 未經額外授權可做 | 必須使用者明確同意 |
|---|---|---|
| 程式與文件 | 檢視、提出方案、在隔離候選工作區編輯與測試 | 推送至遠端、直接修改 main、建立 PR 或合併 |
| 測試 | 沙盒模擬、臨時資料庫、只讀檢查 | 正式 DB 寫入、破壞性測試、停服務、VM 強制程序終止 |
| 系統管理 | 生成供審核的指令與報告 | sudo 變更、刪檔、清理 Docker/Systemd、重啟服務、排程長期運行 |
| 交付 | 檢查差異、製作本機候選包 | 正式發布、部署、替換既有正式版本 |

此倉庫初始化文件已由使用者要求直接上傳；**不代表未來每次變更均獲得永久寫入授權**。

## 專案固定背景
- Ubuntu UEM Lite ZERO-OPS Rev7：單一管理員 WebCLI；PHP Web API、Python Worker、Broker、Systemd、Cloudflare Tunnel。
- 正式 DB 架構：SQLite + WAL（取代 MySQL）；同時最多 1 Task。
- UI 有 Tab 終端機與分類指令操作；Tab 導航依 [40章 HTML 計畫書](plan/Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html)，不得憑猜測自行改動。
- CLI 輸出可複製；大量執行輸出以 Markdown 下載。取消 JSON 作為人用報告下載格式（API 回應格式依契約，不受此限制）。
- 每個執行腳本提供即時 RUNNING/PASS/FAIL/BLOCKED/SKIP、百分比或批次數、耗時、Exit Code、逾時及 Markdown 報告。
- 報告預設用戶 `~/Downloads`，不得寫入他人專案或永久覆蓋正式資料。

## 每次 AI 開發流程
1. **確認基線**：讀 README、rule.md、正式 HTML、最近決策、Git SHA；列出已知事實與未知。
2. **先做風險盤點**：確認影響 Web／Worker／Broker／DB／Systemd／UI／跨專案的範圍。
3. **集中實作**：同類問題一次整合，避免每個小缺陷各建新 ZIP、Commit 或 PR；禁止過度工程。
4. **建立真證據**：測試需附環境、指令、版本 SHA、實際輸出、PASS/FAIL/BLOCKED，不得把 mock/單元測試說成真機 E2E。
5. **闸門判定**：對照 [驗收矩陣](plan/ACCEPTANCE_MATRIX.md)；有 P0、BLOCKED 或證據缺失，即不能宣布驗收/部署完成。
6. **交付**：產出變更摘要、文件與完整單一候選包（視授權）、SHA-256、回滾步驟；敏感記錄只留私有位置。
7. **回報**：說明新增、修改、已測、未測、風險、下一步；沒有實際改動就不要虛構 Batch 進度。

## 最高安全要求
- EachCore、iStudy、Shared Modules 等其他專案不得被停止、清理或意外影響。
- 未經同意不得刪 Dev63 等舊版／備份；只在新版本完整驗收、備份確認與使用者明確核准後清理對應舊檔。
- 不提交金鑰、Token、私有環境檔、真實 DB、主機盤點原始輸出、完整內部路徑／服務憑證。
- 公開文件只保留脫敏資訊；必要的實際 IP/端點即使不是密碼，也須提醒其公開性。
- 不得因 CI 綠燈自動結案、不自動合併 PR、不未授權建 PR。

## 專案索引
- [最高規則](rule.md)｜[專案計畫](plan/PROJECT_PLAN.md)｜[決策](plan/DECISIONS.md)
- [原始 40章 HTML](plan/Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html)｜[驗收矩陣](plan/ACCEPTANCE_MATRIX.md)
- [打包規範](package/README.md)｜[版本索引](package/RELEASE_INDEX.md)
- [架構](docs/ARCHITECTURE.md)｜[安全](docs/SECURITY.md)｜[證據](evidence/README.md)
