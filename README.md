# iphoto
用照片記錄心情感受
---
# 📸 iPhoto | 個人攝影空間
一個輕量級、無需複雜建置流程（No-Build）的單頁應用程式（SPA）個人攝影藝廊。
基於 React 與 Firebase 構建，支援 EXIF 自動讀取、地圖軌跡模式與時間軸檢視，完美紀錄生活與旅行的瞬間。

🔗 **[線上展示 (Demo)](https://gavintux.github.io/yi/demophoto.html)**

## ✨ 功能特色

* **無需編譯 (No-Build)**：全站僅需一個 HTML 檔案，透過 CDN 引入 React 與 Tailwind，部署極其簡單。
* **智慧 EXIF 解析**：上傳照片時自動讀取相機型號、鏡頭、光圈、快門、ISO 及 GPS 資訊。
* **多元瀏覽模式**：
* 🖼️ **瀑布流畫廊**：美觀的 RWD 響應式佈局。
* 🗺️ **地圖模式**：整合 Leaflet 地圖，以聚類標記（Clustering）顯示拍攝足跡。
* 📅 **日曆視圖**：透過年/月視圖回顧特定日期的攝影作品。


* **後台管理系統**：內建輕量級管理介面，支援拖曳上傳、編輯標籤、撰寫拍攝/觀賞心得及刪除照片。
* **分享與下載**：支援自動浮水印下載，以及一鍵複製 Obsidian 語法或分享至社群媒體。

## 🛠 技術架構

本專案採用現代化的前端技術棧，但保持了極簡的單一檔案架構：

* **Frontend**: React 18 (via CDN), Babel Standalone
* **Styling**: Tailwind CSS (via CDN)
* **Backend / Database**: Google Firebase (Firestore & Storage)
* **Authentication**: Firebase Auth (Anonymous Login) + 前端簡易 Passcode 驗證
* **Maps**: Leaflet.js & OpenStreetMap
* **Utilities**: Exif.js (中繼資料讀取), Lucide React (圖標庫)

---

## 🚀 安裝與詳細設定指南

由於本專案為單一 HTML 架構，您不需要執行 `npm install`。請依照以下步驟設定 Firebase 後端服務。

### 步驟 1：取得程式碼

Clone 本專案或直接下載 `index.html` (原 `linkcphoto.html`)。

```bash
git clone https://github.com/gavintux/linkc-photo-space.git

```

### 步驟 2：建立 Firebase 專案並取得金鑰

本程式依賴 Firebase 的 Authentication、Firestore 和 Storage，缺一不可。

1. 前往 [Firebase Console](https://console.firebase.google.com/) 並登入 Google 帳號。
2. 點擊 **"新增專案"**，輸入名稱（如：`Linkc Photo Space`）並建立。
3. 進入專案首頁，點擊畫面中央的 **Web 圖示 (`</>`)**。
4. 輸入應用程式暱稱，**不要勾選** "Firebase Hosting"，點擊 **"註冊應用程式"**。
5. 複製畫面上的 `const firebaseConfig = { ... };` 內容備用。

### 步驟 3：開通 Firebase 服務 (⚠️ 關鍵步驟)

您必須在 Firebase 後台手動啟用以下服務，程式才能運作：

* **啟用 Authentication (身份驗證)**
* 左側選單：**建構** > **Authentication** > **開始使用**。
* 進入 **登入方式** 分頁，選擇 **匿名 (Anonymous)**。
* 切換為 **啟用 (Enable)** 並儲存。


* **啟用 Firestore Database (資料庫)**
* 左側選單：**建構** > **Firestore Database** > **建立資料庫**。
* 位置建議選擇 `asia-east1` (台灣) 或鄰近節點。
* 安全規則選擇 **以測試模式啟動 (Start in test mode)** 並建立。


* **啟用 Storage (檔案儲存)**
* 左側選單：**建構** > **Storage** > **開始使用**。
* 安全規則同樣選擇 **以測試模式啟動 (Start in test mode)** 並完成。



### 步驟 4：設定 HTML 檔案

使用文字編輯器打開 `index.html`，找到頂部的 `SITE_CONFIG` 區塊，填入您的資訊：

```javascript
const SITE_CONFIG = {
    // 1. 網站基本資訊
    seo: {
        title: "您的網站標題",
        author: "您的名字",
        // ... 其他 SEO 設定
    },

    // 2. Firebase 設定 (填入步驟 2 取得的資訊)
    firebase: {
        apiKey: "您的_API_KEY",
        authDomain: "您的專案ID.firebaseapp.com",
        projectId: "您的專案ID",
        storageBucket: "您的專案ID.appspot.com",
        messagingSenderId: "...",
        appId: "...",
        measurementId: "..."
    },
    
    // 3. 系統設定
    system: {
        collectionName: 'i_photos_v1',
        adminPasscode: '123456', // <--- 請務必修改此管理員密碼
        itemsPerPage: 12,
    },
    // ...
};

```

### 步驟 5：部署

將修改後的 HTML 檔案上傳至 **GitHub Pages**、**Vercel** 或任何靜態網頁空間即可使用。

---

## 📖 使用說明

### 登入管理員模式

1. 點擊網頁左上角的 **鎖頭圖示 (🔒)**。
2. 輸入您在 `SITE_CONFIG` 中設定的 `adminPasscode` (預設為 `123456`)。
3. 登入成功後，右上角會出現 **"新增"** 按鈕，鎖頭會變為登出圖示。

### 上傳照片

1. 點擊 **"新增"** 按鈕進入上傳頁面。
2. 選擇照片檔案（系統會自動讀取 EXIF 與 GPS）。
3. 若有 GPS 資訊，系統會自動轉換為地址（國家/城市）。
4. 填寫標籤與心得後，點擊 **"發佈照片"**。

## 📸 截圖預覽

| 瀑布流畫廊 | 地圖模式 |
| --- | --- |
|  |  |
| *響應式圖片展示與過濾* | *基於 GPS 資訊的足跡地圖* |

| 日曆回顧 | EXIF 燈箱 |
| --- | --- |
|  |  |
| *直觀的時間軸瀏覽* | *詳細的拍攝參數資訊* |

## 📂 專案結構

```
/
├── iPhoto.html       # 核心檔案 (包含 React 邏輯、樣式與設定)
└── README.md        # 說明文件

```

## 🗺 未來規劃

* [ ] 支援多張照片批次上傳
* [ ] 增加暗色模式 (Dark Mode) 切換開關
* [ ] 效能優化：實作虛擬滾動 (Virtual Scrolling) 以支援大量照片
* [ ] 增加相簿分類功能 (Albums)

## 🤝 貢獻指南

歡迎任何形式的貢獻！如果您有好的想法：

1. Fork 本專案
2. 創建您的 Feature Branch (`git checkout -b feature/AmazingFeature`)
3. 提交您的變更 (`git commit -m 'Add some AmazingFeature'`)
4. 推送到分支 (`git push origin feature/AmazingFeature`)
5. 開啟 Pull Request

## 📄 授權聲明

本專案採用 [MIT License](https://www.google.com/search?q=LICENSE) 授權。

## 📮 聯絡與贊助

如果您喜歡這個專案，歡迎透過以下方式聯繫作者或給予支持：

* **作者**: Linkc
* **個人網站**: [Linkc Portal](https://gavintux.github.io/yi/)
* **Blog**: [Linkc's Blog](https://2blog.ilc.edu.tw/linkc/)

<a href="[https://www.buymeacoffee.com/gavintux](https://www.buymeacoffee.com/gavintux)" target="_blank">
<img src="[https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png](https://www.google.com/search?q=https://cdn.buymeacoffee.com/buttons/v2/default-yellow.png)" alt="Buy Me A Coffee" style="height: 60px !important;width: 217px !important;" >
</a>
