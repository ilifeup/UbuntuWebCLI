# evidence/ — 驗收證據索引（僅脫敏）

## 建議結構
```text
evidence/
├── README.md
└── YYYY-MM-DD_<gate-id>/
    ├── summary.md          # 脫敏摘要
    └── checksums.txt       # 不含機密的交付檔案 checksum
```

## 原始證據保存規則
- 實際 `Host_Gates.md`、`console.log`、DB diff、systemctl/journal 輸出先留在**使用者 Ubuntu 私有資料夾**。
- 公開摘要保留測試版本、日期、環境類型、Gate、PASS/FAIL/BLOCKED、Exit Code、是否存在完整原始記錄；先去除名稱/IP/路徑/Token/個資。
- 不得用整理後的摘要「重建」未發生的 PASS。證據缺失時維持 NOT_RUN 或 BLOCKED。
- 每項 P0 必須附可拋棄 VM 的真實 PID/cgroup/鎖與任務狀態核對結果，但原始細節不公開。
