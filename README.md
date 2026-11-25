# 我的記帳本（GitHub Pages 版）

網頁放在 GitHub Pages，資料存在你自己的 Google 試算表，用 Google 帳號登入。

- `index.html`：記帳本本體
- `config.js`：設定檔，要填兩個值（Google 用戶端 ID、試算表 ID）

> 程式碼是公開的，但**資料不會公開**。資料只存在你的 Google 試算表，只有你共用的 Google 帳號能讀取。config.js 裡的兩個 ID 被別人看到也沒關係。

---

## 一、準備試算表

1. 如果你已經架過 Apps Script 版，**直接用同一份試算表**。資料格式一樣，不用重新匯入。
2. 如果還沒有，到 Google 雲端硬碟新增一份空白試算表，取名「我的記帳本」。
3. 打開試算表，網址長這樣：
   `https://docs.google.com/spreadsheets/d/這一長串就是試算表ID/edit`
   把中間那一長串複製下來，等一下貼進 config.js 的 `sheetId`。
4. 家人要看的話：右上角「共用」→ 輸入家人的 Gmail → 權限選「檢視者」。

## 二、在 Google Cloud 建立登入用的「用戶端 ID」（只要做一次）

1. 打開 https://console.cloud.google.com ，用你的 Google 帳號登入。
2. 上方「選取專案」→「新增專案」，名稱打「我的記帳本」→ 建立。
3. 左邊選單「API 和服務」→「程式庫」→ 搜尋 **Google Sheets API** → 按「啟用」。
4. 左邊選單「API 和服務」→「OAuth 同意畫面」（新版介面叫「Google Auth Platform」）：
   - 應用程式名稱：我的記帳本；使用者支援電子郵件：選你自己
   - 目標對象 / 使用者類型：選「**外部**」
   - 發布狀態維持「**測試中**」，不用送審
   - 「測試使用者」：按「新增使用者」，加入**你自己的 Gmail** 和**要看帳的家人 Gmail**（沒加的人無法登入）
5. 左邊選單「用戶端」（或「憑證」→「建立憑證」→「OAuth 用戶端 ID」）：
   - 應用程式類型：**網頁應用程式**
   - 「已授權的 JavaScript 來源」→ 新增：`https://你的GitHub帳號.github.io`
     （只要到 `.github.io` 為止，後面不要加斜線或資料夾名稱）
   - 建立後會看到「用戶端 ID」，長得像 `123456789-xxxx.apps.googleusercontent.com`，複製下來。

## 三、填好 config.js

用 Mac 的「文字編輯」打開 config.js（打開前先到「格式」選單選「製作純文字」），把兩個值貼進引號裡：

```js
window.APP_CONFIG = {
  clientId: '123456789-xxxx.apps.googleusercontent.com',
  sheetId: '你的試算表ID'
};
```

## 四、發佈到 GitHub Pages（用網頁操作，不需要終端機）

1. 登入 https://github.com → 右上角「＋」→「New repository」
   - Repository name：例如 `finance`
   - 選 **Public**（免費帳號的 GitHub Pages 需要公開的 repository）
   - 按「Create repository」
2. 在新的 repository 頁面按「uploading an existing file」，把 `index.html`、`config.js`、`README.md` 拖進去 → 按「Commit changes」。
3. 上方「Settings」→ 左邊「Pages」→ Source 選「Deploy from a branch」→ Branch 選 `main`、資料夾選 `/ (root)` → Save。
4. 等 1～2 分鐘，頁面上方會出現網址：`https://你的GitHub帳號.github.io/finance/`

### 習慣用終端機的話

```bash
cd ~/Downloads/finance        # 放 index.html、config.js 的資料夾
git init
git add index.html config.js README.md
git commit -m "我的記帳本"
git branch -M main
git remote add origin https://github.com/你的GitHub帳號/finance.git
git push -u origin main
```

推上去之後，一樣到 Settings → Pages 開啟。

## 五、第一次打開

1. 打開網址 → 按「用 Google 帳號登入」。
2. 會看到「**Google 尚未驗證這個應用程式**」：這是自己做的工具都會出現的，按「繼續」。
3. 勾選允許「查看、編輯、建立及刪除你的 Google 試算表」→ 繼續。
4. 如果試算表是新的、還沒有資料：按「建立預設設定」，再到「設定 → 資料備份與核對 → 匯入備份…」，選你從舊版匯出的 .json 檔。
5. 手機：用 Safari 打開網址 → 分享 →「加入主畫面」。

## 使用上要知道的事

- **登入**：每次打開要按一次「用 Google 帳號登入」。登入大約維持 1 小時。快到期時，你點畫面任何地方會自動續期，可能會閃一下 Google 的小視窗。如果真的過期，上方會出現「重新登入」，還沒存的資料會在登入後補存，不會不見。
- **家人**：家人要同時被加進「測試使用者」**和**試算表的「共用（檢視者）」，用自己的帳號登入後只能看、不能改。想讓他也能記帳，就把試算表權限改成「編輯者」。
- **更新程式**：之後要改功能，只要在 GitHub 上把 `index.html` 換成新檔（打開檔案 → 右上角「…」→「Upload files」或直接覆蓋上傳）→ Commit，一分鐘後網址就會更新。
- **Apps Script 版**：如果你也架了 Apps Script 版，兩邊用的是同一份試算表，資料是同一份。習慣後可以只留 GitHub 版。
- **試算表裡的「data」工作表**：每一列是一筆資料，請不要手動修改或排序。
