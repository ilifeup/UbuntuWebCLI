# rule.md — UbuntuWebCLI 最高專案規則

狀態：ACTIVE（2026-11-06）｜適用範圍：本倉庫全部 AI、開發者、CI、腳本、交付與部署操作。

> 衝突處理：安全與資料保全優先；使用者最新明確授權優先於過時決議，但不可把不具備的驗收證據說成 PASS。此檔與 AGENTS.md 衝突時，以本檔為準；不等於允許跳過不可逆操作的確認。

## R01 專案隔離
1. 僅管理 UbuntuWebCLI/UEM Lite 範圍，不修改 EachCore、iStudy、Shared Modules 或其他容器、Systemd unit、DB、目錄。
2. 不執行全域 `docker system prune`、模糊萬用字元 rm、全域 kill、重設 firewall、SSH、Cloudflare Tunnel 等跨專案動作。
3. 新舊版並行期間先保留舊版與備份；完整驗收且使用者**明確核准**後才允許精準刪除指定舊版。

## R02 寫入權限
1. 無本次任務的明確授權，不得在 GitHub 建立 PR、推 main、合併 PR、發 Release、改 GitHub 設定。
2. 不得無授權操作正式主機 sudo、service、Docker 或正式 DB；所有刪除先盤點、勾選、確認，再執行。
3. 「繼續」表示繼續已授權的開發／審查，不推定授權正式部署或刪除。

## R03 資料庫與任務
1. SQLite + WAL 是唯一正式資料庫；Migration 專責初始化 WAL，API/Worker 連線驗證而非每次重設 journal_mode。
2. Max concurrent Task=1。所有寫入遵循唯一性約束、transaction、busy_timeout、失敗 rollback 與可恢復狀態。
3. `--dry-run` 必須真正唯讀：不得調整 Schema、正式資料、持久 metadata；WAL/SHM 副作用要被驗證及明示。
4. 不可在真實正式 DB 上執行壓測、破壞式 migration 測試或清理。

## R04 執行安全與 P0
1. 所有執行命令必須有權限檢查、允許清單／Action Registry、記錄與防注入措施；root 為顯式核准的高風險模式。
2. Worker/Broker 崩潰恢復 P0 未結案前**禁止正式上線判定**。需以可丟棄 VM 驗證 SIGKILL、cgroup 下所有子程序、資源鎖及任務回復。
3. VM 破壞性 Gate 必須在輸出檔／服務變更前先完成雙重確認和 VM 環境驗證，不可直接在多專案 Ubuntu 主機上執行。

## R05 UI／API 契約
1. Tab 導航、指令分類、手機 Chrome 操作與 API 契約以正式計畫書和決策為準，避免不必要重構。
2. 保留可複製 CLI 輸出；供使用者下載的詳細報告採 Markdown，不新增 JSON 報告下載功能（API JSON 不受限）。
3. 對外服務經 Cloudflare Tunnel/Access；不得把 Tunnel 視為免驗證機制。金鑰與權限採最小權限。

## R06 測試與驗收闸門
1. 每次修正至少有語法／單元／整合／回歸的適當測試證據；P0 必須真實 VM，不得僅靠 mock。
2. 狀態僅用 PASS、FAIL、BLOCKED、SKIP、NOT_RUN、OPEN；BLOCKED 不可當 PASS，測試工具 PASS 不等於產品驗收 PASS。
3. 必須保存所測 commit、環境、指令、起訖時間、Exit Code、結果與原始記錄路徑；公開版本僅放脫敏摘要。
4. PHP/Python 200 輪 SQLite 壓測、Migration 唯讀、Systemd VM P0、實際 Worker/Broker 恢復皆列為正式 Gate。
5. CI 綠燈、ZIP 完整、單元測試通過均**不能自動結案**。完成狀態須見 [驗收矩陣](plan/ACCEPTANCE_MATRIX.md)。

## R07 開發、交付與打包
1. 集中相關修正，避免過度開發、重複測試、冗餘腳本、零碎 Batch/PR。先確認現有工具可重用。
2. GitHub 保存可審查、可追蹤的原始碼與規格。正式安裝成品在已批准的 Release/ package 版本目錄；避免雲端組譯大型產物。
3. 原始碼 ZIP 不含 `dist/`、`node_modules/`、`.deb`、`.pytest_cache/`、`__pycache__/`、資料庫、機密及大量測試日誌。發行成品另附 SHA-256。
4. 一次交付完整整合候選版本；附版本、commit、manifest、差異、測試報告、限制、備份/回滾；交付前核對 ZIP 內容。
5. `package/` 不應不受控地存所有中間 ZIP。每次核准的交付記錄在索引，較大的二進位使用 GitHub Releases Assets／私有儲存。
6. 新程式不應覆蓋原有版本、其他專案、正式資料庫；編譯／測試在隔離工作目錄。

## R08 即時進度與報告
所有長作業提供 RUNNING/PASS/FAIL/BLOCKED/SKIP、階段、百分比／計數、耗時、timeout、Exit Code；每輪產生 Markdown 報告，預設 `~/Downloads`。不應將私密 stdout 全文上傳公開 GitHub。

## R09 保密、審計與發布
- 本倉庫為**公開**。提交前掃描 API token、金鑰、Cookie、個資、內網位址、實際路徑、正式資料和未遮蔽 log。
- 原始設計 HTML 含端點示例，公開檔案會被任何人讀取；端點不等於憑證，但需確認無敏感權杖。
- 不把 `noindex` 當成防止訪問／加密機制；真正保密資料只能放私有儲存庫或受控空間。
- 使用者核准版本和正式部署，是兩個獨立的決策。

## R10 完成定義
**P0 CLOSE + 所有必須 Gate PASS + 安全與回滾審核 PASS + 使用者明確批准**，才可標示「正式驗收完成」。其餘只能標「候選版」「部分通過」「待實測」。
