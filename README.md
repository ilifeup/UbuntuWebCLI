# UbuntuWebCLI — UEM Lite Rev7

> 單一管理員的 Ubuntu Web CLI / UEM 管理系統。**目前是整合候選版（FastTrack v5），不是正式部署驗收通過版。**

## 先看這三份
1. [AGENTS.md](AGENTS.md)：提供 ChatGPT／Claude／Codex 等 AI 工具的操作指引（標準名稱，避免 Windows 大小寫衝突）。
2. [rule.md](rule.md)：專案最高規則、安全紅線、開發與驗收 Gate。
3. [plan/PROJECT_PLAN.md](plan/PROJECT_PLAN.md)：需求、架構、里程碑、已知阻礙與完成定義。

## 資料分類
| 路徑 | 用途 | 規則 |
|---|---|---|
| `AGENTS.md` | AI 接手、工作流程及開發約定 | 從此檔開始；服從 `rule.md` |
| `rule.md` | 最高專案規則 | 變更須有明確使用者授權；安全優先 |
| `plan/` | 專案計畫書、決策、驗收矩陣、里程碑 | 決議與驗收證據分開 |
| `package/` | 已核准公開的版本交付物與 manifest 索引 | 每次交付一個完整候選版本，不累積快取與重複大包 |
| `docs/` | 技術架構、部署與安全說明 | 文件不能假冒實測證據 |
| `evidence/` | 脫敏的驗收證據索引 | 原始敏感記錄保留在 Ubuntu 本機 |
| `ubuntu_admin_webapp/` | **預留**正式候選原始碼存放路徑 | 尚未從舊 ZIP 完成來源審計及正式匯入 |

Git 無法保存空目錄，目錄會透過 README 文件建立。不要誤認目前 `ubuntu_admin_webapp/` 已上傳。

## 目前已知驗收狀態（2026-11-06）
- PHP 安全測試：42/42 PASS（既有沙盒測試紀錄）。
- Python 整合回歸：40/40 PASS；指定回歸 14/14 PASS（既有沙盒測試紀錄）。
- Python 搶佔競爭：100/100 PASS（既有沙盒測試紀錄）。
- PHP/Python 混合 SQLite 200 輪：**BLOCKED**，需有 `pdo_sqlite` 的 Ubuntu 執行。
- Migration `--dry-run` 唯讀驗收：**BLOCKED**，需 Ubuntu/PHP SQLite 實測。
- Systemd Worker/Broker 子程序與資源鎖崩潰恢復：**P0 OPEN**，需隔離 VM 完整驗收。

**CI 綠燈／單元測試 PASS／ZIP 可解壓，均不代表整合驗收完成或准許部署。**

## 開發原則
- 單一管理員、SQLite WAL 唯一正式資料庫、同時最多執行一個 Task。
- 無明確授權：**不得建立 PR、合併、部署、刪除資料或變更其他專案**。
- EachCore、iStudy 及其他服務完全隔離，禁止跨專案清理 Docker／Systemd／資料目錄。
- 完整原始碼／驗收工具與安裝成品分開交付；不在雲端拼湊大型安裝包。
- 重要操作留下即時 `RUNNING/PASS/FAIL/BLOCKED` 進度、Exit Code、Markdown 報告。

## 版本入口
- [計畫書](plan/PROJECT_PLAN.md)
- [驗收清單](plan/ACCEPTANCE_MATRIX.md)
- [決策與變更](plan/DECISIONS.md)
- [交付包規範](package/README.md)
- [交付索引](package/RELEASE_INDEX.md)
- [架構說明](docs/ARCHITECTURE.md)
- [本機證據規範](evidence/README.md)

本公開儲存庫不應包含憑證、Token、私鑰、資料庫、用戶資料、實際主機路徑快照、敏感日誌。