# 2026-08-03 工作進度

## 今天完成的工作

**建立 GitHub workspace 倉庫，把整個工作區域納入版控**

1. 在 `G:\Mybazaar\mybazaar_app` 新增 `CLAUDE.md`，把工作進度紀錄慣例（`markdown/YYYY/MM月/YYYYMMDD/progress.md`）與共用資源規則正式寫成專案設定檔（一般檔案，VS Code / GitHub 都能直接讀取，不是外部隱藏設定）
2. 確認 `G:\Mybazaar` 底下除了 `mybazaar_app`，還有一份既有的 `mybazaar20` 複本（已經是獨立 git repo，指向 `chenyanxun5819/mybazaar20`，含 `.env` 等機密檔案），跟使用者確認後決定兩者都要納入同一個新 workspace repo
3. 安裝 GitHub CLI（`gh` 2.97.0，透過 winget）；確認 git credential manager 既有憑證可正常運作（不需要額外登入就能 push）
4. 使用者手動在 GitHub 建立空的 private repo：`chenyanxun5819/mybazaarWorkspace`
5. 在 `G:\Mybazaar` 執行 `git init`，把 `mybazaar20` 以 **git submodule** 方式接入（`git submodule add`，對應既有 remote），**不複製**任何檔案內容，只記錄指向該 repo 特定 commit 的參照——確認 `.env`、`.firebase` 等機密檔案沒有進到新倉庫
6. 把 `mybazaar_app` 以一般檔案加入、commit、`git push -u origin main`，push 成功

**結果**：`https://github.com/chenyanxun5819/mybazaarWorkspace` 現在同時包含 `mybazaar_app`（一般檔案）與 `mybazaar20`（submodule 參照）。之後 clone 這個 workspace repo 要另外執行 `git submodule update --init` 才會抓到 `mybazaar20` 的實際內容。

## 卡住的地方（延續前一篇，尚未解決）

`flutterfire configure` 綁定 `mybazaar-c4881` 仍卡在 `firebase.googleapis.com` 連線逾時（見 `markdown/2026/07月/20260707/progress.md`）。今天沒有再嘗試，狀態不變。

## 接下來要做的工作

- [ ] 排除 `firebase.googleapis.com` 網路連線問題，重跑 `flutterfire configure`
- [ ] 把 artifact 的配色/字體 token 轉成 Flutter `ThemeData`
- [ ] 建立角色導向路由骨架（merchant / cashier / teamLeader 三個殼畫面）
- [ ] `flutter run` 驗證可在裝置/模擬器開啟白底+粉藍主題畫面
- [ ] 之後階段（登入身份、RFID/藍牙、現金對帳、點數分配、整合測試）見計劃書 `C:\Users\wes chen\.claude\plans\groovy-juggling-deer.md`
