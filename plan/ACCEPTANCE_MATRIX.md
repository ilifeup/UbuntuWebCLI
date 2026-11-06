# Rev7 驗收矩陣（禁止假 PASS）

狀態定義：`PASS` = 具指令與原始記錄的真實測試通過；`BLOCKED` = 環境／依賴阻礙；`OPEN` = 問題未解；`NOT_RUN` = 尚未執行；`FAIL` = 已知不符合；`SKIP` = 有正當跳過理由。

| ID | 驗收 Gate | 必須的實際證據 | 目前狀態 | 阻擋部署 |
|---|---|---|---|---|
| SEC-01 | PHP 安全回歸 | 完整測試結果、版本、環境 | PASS（歷史 42/42） | 是 |
| PY-01 | Python 整合回歸 | 完整回歸記錄 | PASS（歷史 40/40） | 是 |
| WK-01 | Worker 搶佔唯一性 | 100 輪真實並發 claim、競爭條件及狀態 | PASS（歷史 100/100） | 是 |
| SQL-01 | PHP/Python SQLite WAL 同時寫入 | Ubuntu `pdo_sqlite`、200/200、鎖等待及 DB 交易證據 | **BLOCKED** | **是** |
| SQL-02 | Migration `--dry-run` 唯讀 | DB、schema、檔案副作用前後比較 | **BLOCKED** | **是** |
| P0-01 | Systemd SIGKILL 清理整個 cgroup | 可丟棄 VM、真實程序 PIDs、session、cgroup、無孤兒 | **OPEN** | **是** |
| P0-02 | Worker/Broker 任務狀態與資源鎖恢復 | 崩潰前後 DB、鎖、任務 transitions、重試 | **OPEN** | **是** |
| E2E-01 | Web/API → Worker → CLI → Markdown | 端到端成功/拒絕/逾時、手機或模擬客戶端 | NOT_RUN | **是** |
| AUTH-01 | 分層權限／root／Access/MFA | 未授權禁止、權限升級阻擋、憑證失效 | NOT_RUN | **是** |
| UI-01 | Tab 導航與手機 Chrome | 實際頁面、指令集、複製／下載 | NOT_RUN | 視發布範圍 |
| OPR-01 | 不影響 EachCore/其他專案 | 執行前後服務與資料核對 | NOT_RUN | **是** |
| PKG-01 | 乾淨包、SHA-256、回滾 | 可重複建置、檔案 manifest、回復操作 | NOT_RUN | **是** |
| USER-01 | 使用者明確批准 | 對應版號與驗收證據的批准記錄 | NOT_RUN | **是** |

**歷史 PASS 不代表已與最新 v5 候選程式做過完整整體回歸。**

## Gate 執行安全
1. 主機 Host Gate 使用獨立可拋棄 DB，不操作現有正式 DB；需 `pdo_sqlite`。
2. Systemd VM Gate 必須雙確認和可丟棄 VM；不可在多專案 Ubuntu 主機執行。
3. 要用真實 Worker/Broker 而非純 mock 或測試子程序，證明 P0-01/P0-02。
4. `BLOCKED` 應採非 0 Exit Code 並於 `summary.md` 呈現。
5. 每輪附版本 SHA／系統條件／時間／原始路徑／判定標準；僅脫敏後對外發布。

## 完成判定
必要 Gate 全 PASS ＋ P0 關閉 ＋ 安全核查/回滾 PASS ＋ 使用者批准，否則僅可標示候選版。
