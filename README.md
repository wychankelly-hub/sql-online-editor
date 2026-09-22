# SQL Online Editor & Classroom

一個在瀏覽器內運行的 SQLite 編輯器，附設課堂模式，供學生即時把 SQL 答案提交給老師。適用於 HKDSE ICT 選修部分「數據庫」的課堂練習。

**網站：** https://wychankelly-hub.github.io/sql-online-editor/

---

## 功能

**Query Runner 分頁**
- 開啟 SQLite 資料庫檔案（`.db` / `.sqlite` / `.sqlite3`），可拖放或選擇檔案
- 左欄列出所有資料表及記錄數目，點擊即可檢視內容
- 撰寫及執行 SQL（快捷鍵：`Ctrl + Enter` / `Cmd + Enter`）
- 點擊欄位名稱可排序結果；`NULL` 值會特別標示
- 自動把 `MINUS` 轉換為 SQLite 支援的 `EXCEPT`
- 資料庫只在瀏覽器內處理，不會上載到任何伺服器

**Classroom 分頁**
- 老師建立 4 位數字的 session code，學生輸入後即可加入
- 學生提交的 SQL 即時顯示在老師畫面，可一鍵載入並執行
- 學生無論使用家中 Wi-Fi、流動數據或學校網絡均可連線
- 老師頁面顯示在線學生人數
- 重新整理頁面後可恢復 session，提交紀錄保存在雲端
- 可匯出所有提交紀錄為 JSON
- 結束 session 後，所有學生自動斷線

---

## 使用方法

### 學生

1. 打開網站。如只需提交答案而沒有資料庫檔案，可點擊「Student: join class without a database」。
2. 在 **Query Runner** 分頁撰寫及測試 SQL。
3. 轉到 **Classroom** 分頁，選擇「I am a Student」。
4. 輸入班別學號（例如 `5A-12`）及老師提供的 4 位數字 code，點擊「Connect to Teacher」。
5. 點擊「Copy query from Query Runner」，再點擊「Submit Query to Teacher」。
6. 看到「✅ Submitted」即表示老師已收到。

### 老師

1. 打開網站並載入課堂使用的資料庫檔案。
2. 轉到 **Classroom** 分頁，選擇「I am a Teacher」。
3. 點擊「Create Session Code」，把 4 位數字 code 投影給學生。
4. 學生提交的 SQL 會顯示在「Live Submissions Feed」，點擊「Load & Run in SQL Editor」即可執行。
5. 下課前點擊「Export JSON」保存紀錄，再點擊「End Session」。


---

## 使用限制及注意事項

- **同時連線上限約 100 部裝置**（Firebase 免費方案）。此上限計算的是同一時間在線的裝置，並非每日總人數。老師點擊「End Session」後，所有學生會自動斷線並釋放名額。
- **個人資料：** 建議學生只填寫班別學號，不要填寫全名，以符合《個人資料（私隱）條例》的要求。
- **Session code 只有 4 位數字**，任何人估中 code 都可以看到該節的提交內容。請勿用此工具收集任何敏感資料。
- 老師重新整理頁面後，可點擊「Resume session」恢復；「Delete All Submissions」會永久刪除該節所有紀錄。

---

## 疑難排解

| 情況 | 可能原因及解決方法 |
|---|---|
| 點擊按鈕後顯示「Teacher setup needed」 | `FIREBASE_CONFIG` 尚未填寫，請參考「部署」第 2 步 |
| 「Permission denied」 | Database Rules 未正確發佈，請重新貼上並點擊 Publish |
| 「No response from the server」 | 網絡問題，或學校網絡過濾器封鎖了 `www.gstatic.com` 或 `firebasedatabase.app` |
| 「Session code not found」 | Code 輸入錯誤，或老師已結束 session |
| 「The Firebase library did not load」 | 網絡過濾器封鎖了 `www.gstatic.com` |
| 設定 API key 網站限制後無法使用 | 檢查網址格式是否為 `https://你的帳戶名.github.io/*`，並等候約 5 分鐘讓設定生效 |
| 在本機直接開啟檔案時無法連線 | 已設定網站限制時，本機檔案不在容許名單內。請使用 GitHub Pages 網址，或在限制中加入 `http://localhost/*` 並以本機伺服器開啟 |

---

## 技術資料

- 單一 HTML 檔案，毋須安裝或編譯
- [sql.js](https://github.com/sql-js/sql.js) 1.8.0：以 WebAssembly 在瀏覽器內運行 SQLite
- [Firebase Realtime Database](https://firebase.google.com/docs/database)（compat SDK 10.12.2）：課堂模式的即時資料傳送
- 資料庫檔案只在使用者瀏覽器內處理；只有學生提交的 SQL 文字、名稱及時間會傳送至 Firebase
