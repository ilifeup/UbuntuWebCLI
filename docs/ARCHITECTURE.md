# 技術架構（Rev7 候選基線）

```text
Android App / HTTP Shortcuts / Mobile Chrome
             │
  Cloudflare Tunnel + Access
             │
     Web UI / PHP API
             │
      SQLite WAL DB  ←── PHP Migration
             │
        Python Worker  (max 1 Task)
             │
      Broker / Action Registry
             │
    Systemd cgroup / Ubuntu CLI
             │
       Markdown report
```

## 資料與流程契約
1. PHP API 驗證身份與權限，建立任務；DB 交易必須可回滾、使用唯一約束及忙碌等待。
2. Python Worker 唯一搶佔任務；禁止並行 2 Task；DB 初始化 WAL 由 Migration 負責，連線只讀驗證 journal_mode。
3. Broker 限縮 sudo/root 權限與指令允許清單；用 systemd cgroup 管理整個執行樹。
4. 程序遭 `SIGKILL` 時不能留下執行中的子程序、資源鎖或永久 `running` 任務。
5. 使用者可選取複製 CLI 輸出，長輸出提供 Markdown 報告，含指令、執行時間、狀態、Exit Code、全文。
6. Browser Tab 導航與分類操作依 [正式 40章規劃](../plan/Ubuntu_UEM_Lite_ZERO_OPS_Rev7_修復Tab導航_40章完整計畫書_20261105.html) 第 37–39 章；避免在無明確決策下擴張功能。
7. 不受信任的外部輸入不能直接執行 shell；必須固定參數化、權限審核及審計記錄。

## 此處不是最新正式運行系統的事實清單
此文件描述候選設計。Ubuntu 上實際服務名稱、路徑、埠、憑證與版本需由安全脫敏的主機驗收證據確認，不能從計畫推測。
