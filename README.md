# 惠韻牙醫診所 — 助理薪資系統

單一檔案的網頁應用程式，資料存在診所自有的 Firebase 專案。

## 架構

| 層 | 用什麼 | 說明 |
|---|---|---|
| 網頁 | GitHub Pages | 公開網址，但不含任何薪資資料 |
| 登入 | Firebase Auth（Google） | 只放行白名單帳號 |
| 資料 | Cloud Firestore `payroll/state` | 整份狀態以 JSON 字串存在單一文件 |

**這個 repo 裡沒有任何員工姓名或薪資金額。** 資料只存在 Firestore，
第一次啟用時用頁面右上角的「匯入備份」載入 JSON 備份檔。

## 檔案

- `index.html` — 整個系統（HTML／CSS／JS 都在這一個檔案裡）
- `firestore.rules` — Firestore 安全規則，需手動貼到 Firebase 主控台發布

## 部署

1. GitHub → Settings → Pages → Source 選 `main` 分支、`/ (root)`
2. 網址為 `https://<帳號>.github.io/yun_salary/`
3. 到 Firebase 主控台 → Authentication → Settings → 授權網域，
   加入 `<帳號>.github.io`
4. Firestore Database → 規則，貼上 `firestore.rules` 的內容（先把 Email 換成實際帳號）並發布

## 資料備份

頁面右上角「匯出備份」會下載完整的 JSON。建議每月發薪、鎖定月份之後做一次，
存到 Google Drive。Firestore 本身也有排程備份可開，但手動備份是最後一道保險。

## 功能

月薪資表 ｜ 薪資單 ｜ 匯款分配 ｜ 項目查閱 ｜ 特休帳 ｜ 年度統計 ｜ 雇主成本 ｜ 投保合規 ｜ 設定

- 診數制：1 診 = 半個工作日，請假與出勤率皆以診為單位
- 全勤獎金依遲到分鐘數階梯遞減；事假、病假歸零；生理假折半；特休與颱風假不影響
- 已鎖定（已發放）的月份數字全部寫死，日後改設定不會回頭影響歷史
- 儲存前會比對雲端時間戳，發現雲端較新就擋下，不會覆蓋別台裝置的修改
