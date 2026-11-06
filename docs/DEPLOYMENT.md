# 部署與回滾指引（暫不可執行正式部署）

**狀態：DRAFT／BLOCKED。** 本 repo 目前沒有通過必要 Gate 的正式部署包。

## 上線前必備
1. 對照 [驗收矩陣](../plan/ACCEPTANCE_MATRIX.md)，所有部署阻擋 Gate PASS，P0 CLOSED。
2. 記錄完整版本、SHA-256、變更與回滾快照；驗證備份可讀。
3. 確認 EachCore/iStudy/其他專案的 Docker、Systemd、網路與 DB 均不受影響。
4. 使用者對**指定版號**明確批准；不以「繼續」當作部署授權。

## 參考流程（非可直接執行的腳本）
- 隔離 stage 路徑部署候選，不改正式資料。
- 先跑環境自檢、Migration 唯讀檢查與主機/VM 適用驗收。
- 備份、部署、健康檢查與功能煙測；設計明確 rollback。
- 故障時停止指定 UEM 新版服務、回復 UEM 專屬資料；不可停全域 Docker。
- 舊版清理另行列清單，經確認後才執行。

具體 service 名稱、port、部署 root path 應由正式候選原始碼實際核對後再補，不能自行猜測。
