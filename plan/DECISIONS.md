# 決策與變更追蹤

每筆決策記錄 `ID | 日期 | 狀態 | 決策 | 原因 | 影響 | 驗收證據`。未經驗收的測試結果不得寫成「正式完成」。

| ID | 日期 | 狀態 | 決策／理由 |
|---|---|---|---|
| D-001 | 2026-11-06 | ACTIVE | 使用 `AGENTS.md` AI 工具標準入口；`agent.md` 僅導向，不維護重複規則。 |
| D-002 | 2026-11-06 | ACTIVE | 使用 `rule.md` 作為本 repo 最高專案規則；不自動建 PR／合併／發布／部署。 |
| D-003 | 2026-11-06 | ACTIVE | `plan/` 保存原始 HTML 計畫書與決策／驗收文件；歷史版本不可覆蓋或靜默替換。 |
| D-004 | 2026-11-06 | ACTIVE | `package/` 保存每次**核准交付**的索引、manifest、SHA，去除快取；大型包優先 Release Assets。 |
| D-005 | 已決 | ACTIVE | SQLite WAL 唯一正式 DB；最大同時 1 Task。 |
| D-006 | 已決 | ACTIVE | 每個長指令及測試顯示即時進度、耗時與 Markdown 報告。 |
| D-007 | 已決 | ACTIVE | 不能影響 EachCore 或其他專案；刪除舊版須先完成新版本驗收、確認備份和再取得明確同意。 |
| D-008 | 2026-11-06 | OPEN | PHP/Python 200 輪混合驗收與 Migration Dry-run 仍未有 Ubuntu 主機 PASS 證據。 |
| D-009 | 2026-11-06 | OPEN | Systemd 真實 Worker/Broker 崩潰恢復 P0，僅可在隔離 VM 驗收後再決定結案。 |
| D-010 | 2026-11-06 | ACTIVE | 原始 40 章 HTML 保持完整內容、原檔名入庫；後續修訂另立版本，不偷改歷史。 |

## 新決策範本
```markdown
### D-XXX｜YYYY-MM-DD｜PROPOSED / APPROVED / REJECTED
問題：
選項：
使用者明確決定：
影響文件與程式：
驗收要求：
變更/PR/Commit：
```
