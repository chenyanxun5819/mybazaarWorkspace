# mybazaar_app（MyBazaar Flutter App）

## 專案概述

MyBazaar 的 Flutter app 版本，服務現場角色（merchant / cashier / teamLeader），與 web 版（`C:\mybazaar20`，React + Vite）共用同一個 Firebase 專案 `mybazaar-c4881` 的 Firestore 資料庫與 Cloud Functions。第一版只做 Android；iOS 之後再處理（開發機是 Windows，iOS 編譯/上架需要 Mac 或雲端 Mac CI）。

## 工作進度紀錄慣例

**每次工作前先讀最新一篇進度紀錄，每次工作後把當次進度寫入對應日期的檔案。**

路徑規則：`markdown/YYYY/MM月/YYYYMMDD/progress.md`（沿用 `C:\mybazaar20\markdown` 既有的命名慣例）。同一天多次工作，直接在當天的 `progress.md` 追加內容，不要另開新檔。

每篇進度至少要包含：
- 今天完成了什麼
- 卡住的地方（如果有）
- 接下來要做的工作

## 共用資源規則

這個專案**不會**複製任何 `mybazaar20` 的檔案。以下都以 `C:\mybazaar20` 為唯一事實來源，開發前直接去讀該檔案：

- Firestore 資料模型/API 合約：`C:\mybazaar20\firestore最新架构.json`（修改前一定要先讀這份文件，本專案不得複製或修改它）
- Firestore/Storage 安全規則、部署：只透過 `mybazaar20` 部署，本專案不建立自己的 `firestore.rules`
- Cloud Functions：本專案只呼叫既有的 HTTPS endpoint（`processRfidPayment`、`allocatePointsByTeamLeaderHttp`、`submitCashToFinanceHttp` 等），不複製 functions 原始碼

## 目前狀態

詳見最新一篇 `markdown/2026/07月/20260707/progress.md`。簡述：Flutter/Android 開發環境已建置完成、專案骨架已建立（package name `com.mybazaar`），`flutterfire configure` 綁定 `mybazaar-c4881` 卡在網路問題（`firebase.googleapis.com` 連線逾時），待排除後繼續。
