# UbuntuWebCLI — UEM Lite Rev7

**狀態：候選版整合與驗收中（2026-11-06）｜不是已正式部署／驗收的版本。**

此倉庫為 Ubuntu UEM Lite ZERO-OPS Rev7 的**正式規格、治理與後續程式碼基線**，目前先初始化規則與文件，尚未匯入完整候選程式碼或 ZIP。

## 重要入口
1. **[40 章 HTML 完整計畫書](plan/Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html)** — 2026-11-05 原檔（含 Tab 導航修復規劃）。
2. **[rule.md](rule.md)** — 最高安全、開發、驗收與交付規則。
3. **[AGENTS.md](AGENTS.md)** — AI 工具標準指引；[agent.md](agent.md) 為相容入口。
4. **[專案進度計畫](plan/PROJECT_PLAN.md)** / **[驗收矩陣](plan/ACCEPTANCE_MATRIX.md)** / **[決策](plan/DECISIONS.md)**。
5. **[package/ 每版打包索引](package/README.md)** / **[Release Index](package/RELEASE_INDEX.md)**。
6. **[技術架構](docs/ARCHITECTURE.md)** / **[部署](docs/DEPLOYMENT.md)** / **[安全](docs/SECURITY.md)** / **[驗收證據](evidence/README.md)**。

## 建議分類
```text
UbuntuWebCLI/
├── README.md
├── AGENTS.md            # AI 標準入口
├── agent.md             # 相容導引，不維護兩套規則
├── rule.md              # 最高專案規則
├── .gitignore
├── plan/                # 原始 40章 HTML、計畫書、驗收矩陣、決策
├── package/             # 每次核准的交付索引、manifest、SHA；大型包用 Releases
├── docs/                # 架構、安全、部署
├── evidence/            # 脫敏驗收證據索引（原始 log 不公開）
└── ubuntu_admin_webapp/ # 預留：完整候選原始碼待核對及授權匯入
```

## 最新已知 Gate
| 項目 | 已知狀態 |
|---|---|
| PHP 安全測試 | 歷史 42/42 PASS |
| Python 整合測試 | 歷史 40/40 PASS |
| Python Worker 搶佔 | 歷史 100/100 PASS |
| PHP/Python SQLite 200 輪 | **BLOCKED**（需 Ubuntu `pdo_sqlite`） |
| Migration `--dry-run` 唯讀 | **BLOCKED**（需 PHP/SQLite 實測） |
| Systemd Worker/Broker SIGKILL cgroup 復原 | **P0 OPEN**（需隔離 VM） |

CI 綠燈、ZIP 完整、單元測試 PASS **不等同正式驗收**。不得在未經使用者授權的情況下建 PR、合併、正式部署、清理舊版或變更 EachCore 等其他專案。

## 重要提醒
本倉庫為**公開**。40章 HTML 中存在示例網址，包括 UEM API 子網域；請勿在公開檔案寫入 Token、私鑰、密碼、真正的主機盤點或敏感資料。若需保密，應改用私人儲存空間，`noindex` 不等於保密。

Git 目錄不會保留空資料夾，因此目前未存在的 `ubuntu_admin_webapp/` 只是規劃位置，非已上傳程式碼。
