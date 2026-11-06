# package/ — 每次整合交付版本

本目錄不是「每個小修改都打一次 ZIP」的垃圾堆，而是**每個核准交付版本的追蹤入口**。

## 建議命名與結構
```text
package/
├── README.md
├── RELEASE_INDEX.md
├── MANIFEST_TEMPLATE.md
└── Rev7/
    └── README.md     # 每個核准版本建立 Rev7/<版本號>/ manifest + SHA-256
```

示例（**以下是設計範例，不代表已發布**）：
```text
package/Rev7/FastTrack-v5/
├── manifest.md
├── SHA256SUMS
├── release-notes.md
└── UEM_Lite_Rev7_FastTrack_v5.zip  # 僅限公開核准、檔案大小適合 Git
```

## 交付包分類
| 類型 | 內容 | 不可混入 |
|---|---|---|
| SOURCE | 原始碼、必要 Migration、安裝/測試腳本、規格 | `.deb`、`dist/`、`node_modules/`、cache、私鑰 |
| TEST-KIT | Host/VM Gate、隔離資料庫 fixture、驗收規範 | 正式 DB／敏感 log／會對主機破壞的預設模式 |
| RELEASE | 通過 Gate 的部署／安裝成品 | 未核准的候選成品 |
| EVIDENCE | 脫敏摘要與索引 | Token、完整網路/主機盤點、內網連線資料 |

## 每次交付必須填寫
- 版本、日期、來源 commit、內容分類、檔名、SHA-256、大小。
- 需求/決策來源、變更摘要、測試 PASS/FAIL/BLOCKED/NOT_RUN。
- 每個已知 P0/P1、會影響的服務、備份與回滾。
- 上傳位置（GitHub repo path 或 Release Asset）、發佈是否經使用者授權。
- ZIP 解壓檢查、非預期檔案清單、敏感掃描結果。

大型壓縮檔／安裝成品應使用 GitHub Release Assets（或經批准的私人交付），**不要直接用 Git 歷史堆積大量 ZIP**。公開倉庫的 URL 不是資料保密措施。

## 防止重複累積
同一候選批次一次整合，必要時修正版明確標 `-r2` 並標示取代關係；不要只因重新測試就新增一份大包。最終完整驗收前應標 `CANDIDATE`。
