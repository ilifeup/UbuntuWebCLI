# 安全與公開資料分級

此儲存庫為 **PUBLIC**。所有上傳檔案均應視為可被搜尋、複製與長期保存。

## 可公開
- 不含真實金鑰的架構、UI 草圖、通用 API 規格、脫敏的驗收摘要、規則與版本記錄。
- 使用者指定的 Rev7 HTML 原始計畫書（上傳前已檢查常見敏感憑證樣式；其包含一個公開 FQDN API 範例，不代表端點必然開放或安全）。

## 不可公開
- Token、API keys、Cloudflare Access service token、SSH key、密碼、Cookie、.env、DB、私密日誌、裝置 ID/個資、內網拓撲及敏感備份。
- 原始 Host inventory 和 Docker secrets、實際 systemd credentials。
- 任何未經去識別化的 Markdown 全文報告。

## 提交前檢查
1. 搜尋 `password`、`secret`、`token`、`private key`、`Authorization:`、`/home/`、IP/主機名、URI 憑證。
2. 檢查差異及附件，必要時以專用 secrets 掃描工具複查。
3. 機密一旦被 push，刪除檔案無法抹去 Git 歷史，應先旋轉憑證。
4. `<meta name="robots" content="noindex">` 與 robots.txt 不是存取控制。
5. 所有可破壞性 CLI 必須先盤點、預覽、核准後執行；禁止利用系統管理 UI 避開權限稽核。

## 安全事件
疑似憑證外洩：停止進一步公開、旋轉受影響憑證、檢查操作日誌與存取、評估歷史移除。勿在公開 Issue 中貼出秘密。
