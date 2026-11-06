# UEM Lite ZERO-OPS Rev7｜專案概要與進度

版本基線：2026-11-06｜狀態：**FastTrack v5 候選整合版，未正式驗收、未正式部署**

## 一、正式需求來源
[Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html](Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html)

該文件列出 01–39 與 09A，共 40 個主章節，包含：專案目標、Android Flutter／HTTP Shortcuts、Cloudflare Tunnel/Access、API、Job/CLI、權限與安全、DB/Worker、Tab UI、交付批次、P0/P1、驗收及決策追蹤。

## 二、目標與範圍
- 單一管理者透過 Web/手機遠端安全執行經授權的 Ubuntu 指令。
- UI 支持終端機（顯式 root 選擇）及分類指令／三層選單；手機 Chrome 可操作。
- Backend：PHP API + Python Worker + Broker + SQLite WAL；同時間最多 1 Task。
- Cloudflare Tunnel/Access 作為對外網路入口；需獨立身份、授權、審計。
- 即時狀態、可複製輸出、Markdown 全文下載、逾時與錯誤處理。
- 預留 Android Flutter app／HTTP Shortcuts 整合；不因本階段需要而過度擴展。

## 三、功能與工作流
```text
手機 Chrome/Android 客戶端
       │ (Tunnel + Access + API 授權)
       ▼
  PHP Web UI / API ── SQLite WAL ── Python Worker
                                  │
                                  ▼
                         Broker / 指令允許清單
                                  │
                                  ▼
                     Systemd/cgroup/受控 CLI
                                  │
                                  ▼
                         日誌與 Markdown 報告
```

## 四、目前完成與缺口
| 項目 | 已知證據 | 尚需 |
|---|---|---|
| PHP 安全測試 | 42/42 PASS（既有 Batch 68） | 真實 API E2E |
| Python 整合 | 40/40 PASS（既有 Batch 68）、指定回歸 14/14 PASS | 主機端完整整合 |
| Worker 搶佔 | 100/100 PASS（既有 Batch 72） | PHP/Python 對 DB 真實壓測 |
| SQLite WAL/ Migration | 候選修正完成；測試腳本已建立 | 混合 200 輪 PASS、Dry-run 不變更 DB |
| Systemd cgroup/程序崩潰 | P0 單元/契約相關測試 PASS | 可丟棄 VM 的真實 Worker/Broker 故障驗收 |
| 打包與驗收工具 | FastTrack v5 統一 host/vm 入口 | 本機 Ubuntu 實際記錄與正式驗收門檻 |

> 這些 PASS 是先前回合已有的測試紀錄；本次建立倉庫結構**未重新執行全部測試**。

## 五、開發里程碑
1. **M0 基線入庫**：規則、完整 HTML 計畫書、驗收矩陣、版本索引；建立可審計來源。
2. **M1 原始碼核對**：FastTrack v5 來源與依賴、建置腳本、SHA、忽略快取；經核准後匯入。
3. **M2 Host Gate**：Ubuntu 具 `pdo_sqlite` 的隔離測試 DB，Migration Dry-run + 200 輪 PHP/Python 混合併發。
4. **M3 P0 VM Gate**：可丟棄隔離 VM 中的真實 Worker/Broker cgroup 子程序、SIGKILL、鎖/狀態恢復。
5. **M4 完整 E2E**：Web/UI/API/DB/Worker/Broker/報告/權限/重啟回歸與非回歸。
6. **M5 Release Candidate**：單一乾淨候選包、manifest、SHA-256、部署與回滾手冊。
7. **M6 部署核准**：使用者明確批准且驗收矩陣必須 Gate 全 PASS；舊版與備份清理獨立核准。

## 六、避免過度開發
不新增模組直到 P0、SQLite/ Migration Gate 解決；不做重複 ZIP、不因測試腳本 PASS 宣稱產品上線。正式程式和其他專案不可被未經確認的驗收腳本修改。

## 七、輸出與報告
- 每項 Gate 產生 Markdown，包含執行時間、版本 commit、指令與 Exit Code。
- Host/VM 各自保存獨立證據目錄（只在本機）；公開的 `evidence/` 僅放無機密摘要。
- 目前待使用者提供真實 Host/VM Gate 實測證據；在此前持續 OPEN/BLOCKED。
