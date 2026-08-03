# 2026-07-07 工作進度

> 資料夾規則：`markdown/YYYY/MM月/YYYYMMDD/`，沿用 `C:\mybazaar20\markdown` 既有的命名慣例。之後每次工作，都在對應日期資料夾裡新增/更新當天的進度紀錄。

## 專案目標

在 `G:\Mybazaar\mybazaar_app` 建立 MyBazaar 的 Flutter app 版本，服務**現場角色**（merchant / cashier / teamLeader），與 web 版（`C:\mybazaar20`）共用同一個 Firebase 專案 `mybazaar-c4881` 的 Firestore 資料庫與 Cloud Functions。詳細規劃見計劃書：`C:\Users\wes chen\.claude\plans\groovy-juggling-deer.md`。

## 已確認的決策

- 技術框架：Flutter（原生 app，非 PWA / Capacitor）
- 第一版角色範圍：merchant、cashier、teamLeader（現場硬體互動角色）；platform / eventManager / auditor 等後台角色留在 web
- 平台：**先做 Android**（iOS 之後再處理——這台開發機是 Windows，iOS 編譯/上架需要 Mac 或雲端 Mac CI）
- Android package name：`com.mybazaar`
- 視覺風格：白底 + 粉藍（powder blue）配色，已在 artifact 定案（幾何粗體標題字 + 系統內文字 + 等寬 ledger 數字）
- 共用資源策略：**不建立實體共用資料夾**。`C:\mybazaar20\firestore最新架构.json`、`firestore.rules`、`functions/` 都留在 `mybazaar20` 原地當唯一事實來源，Flutter 專案開發前直接讀取該檔案，不複製

## 今天完成的工作

1. **開發環境建置**
   - 安裝 Flutter SDK 3.44.5（`C:\src\flutter`，git clone stable channel，因為 winget/choco 都裝不了，改用 git clone + 手動加 PATH）
   - 安裝 Android SDK command-line tools（`C:\Android\sdk`），含 `platform-tools`、`platforms;android-36`、`build-tools;36.1.0`，並自動同意所有 SDK 授權
   - 排除 Java 版本檢查誤判問題（JDK 25 被舊版檢查腳本誤判為過舊，用 `SKIP_JDK_VERSION_CHECK` + 正確設定 `JAVA_HOME` 解決）
   - `flutter doctor` 確認 Android toolchain 全綠
2. **專案骨架**
   - `flutter create --platforms=android --org com.mybazaar --project-name mybazaar_app` 建立專案於 `G:\Mybazaar\mybazaar_app`
   - 安裝 FlutterFire CLI（`dart pub global activate flutterfire_cli`）
   - 確認 Firebase CLI 已登入 `weschen@mybazaar.my`

## 卡住的地方（未完成）

`flutterfire configure` 要綁定 `mybazaar-c4881` 專案時，卡在「Fetching available Firebase projects...」逾時失敗（`firebase.googleapis.com`、`www.googleapis.com` 連線逾時，但 `firestore.googleapis.com`、`github.com`、`www.google.com` 正常）。已手動中止指令，**沒有**誤建立新的 Firebase 專案，也還沒產生 `firebase_options.dart` / `google-services.json`。這是這台機器/網路對特定 Google API 網域的連線限制，跟稍早遇到的 `dl.google.com` 連不上是同類但不同網域的問題。使用者已知悉，決定先暫停，待網路問題排除後再繼續。

## 接下來要做的工作（依計劃書分階段）

- [x] 階段 0：風格定案（白底 + 粉藍 artifact）
- [ ] 階段 1：Flutter 專案骨架 **← 目前進度，卡在 `flutterfire configure` 網路問題**
  - [x] `flutter create`
  - [ ] `flutterfire configure` 綁定 `mybazaar-c4881`（被網路問題卡住）
  - [ ] 把 artifact 的配色/字體 token 轉成 Flutter `ThemeData`
  - [ ] 建立角色導向路由骨架（merchant / cashier / teamLeader 三個殼畫面）
  - [ ] `flutter run` 驗證可在裝置/模擬器開啟白底+粉藍主題畫面
- [ ] 階段 2：登入與身份（對接 `loginUniversalHttp` / `sendOtpHttp` / `verifyOtpHttp` / `loginWithPin`）
- [ ] 階段 3：RFID / 藍牙串接（merchant）（對接 `processRfidPayment`、`confirmMerchantPayment` 等）
- [ ] 階段 4：現金對帳（cashier）（對接 `getCashierStats`、`submitCashToFinanceHttp`、`confirmCashSubmission`）
- [ ] 階段 5：點數分配（teamLeader）（對接 `getTeamLeaderDashboardDataHttp`、`allocatePointsByTeamLeaderHttp`）
- [ ] 階段 6：整合測試 + 上架準備（Android 內部測試 → 正式送審；iOS 之後再排）

## 恢復時的第一步

1. 確認網路可連上 `firebase.googleapis.com`（不逾時即可，不一定要回 200）
2. 在 `G:\Mybazaar\mybazaar_app` 執行：
   ```
   flutterfire configure -y --project=mybazaar-c4881 --platforms=android --android-package-name=com.mybazaar --account=weschen@mybazaar.my
   ```
3. 成功後繼續做 ThemeData 主題檔與角色路由骨架
