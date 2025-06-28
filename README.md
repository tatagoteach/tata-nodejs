CreatorVerse - 多重身份創作者生態系平台 API 
歡迎來到 CreatorVerse！這不僅僅是一個專案，而是一個功能完整、可營運、可擴展的多重身份創作者生態系平台。本文件將引導您了解此專案的架構、功能、設定與部署流程。

專案總覽與核心理念
CreatorVerse 的核心理念是**「從使用者到平台創作者」**。我們打破了傳統使用者模型的限制，建立了一個允許任何使用者從單純的「消費者」無縫切換為多重身份「創作者」的生態系統。

在這個平台上，同一個使用者帳號，可以根據其選擇，動態開通並同時擁有多種身份，實現多元化的商業模式：

成為老師 (Instructor)： 建立並銷售高品質的線上課程。

成為店家 (Vendor)： 開設自己的品牌商店，銷售實體商品。

成為服務提供者 (Service Provider)： 例如美容師、顧問、教練，提供可線上預約的專業服務。

我們的目標是提供一個統一、強大且靈活的後端基礎設施，支撐起這個複雜而富有活力的創作者經濟生態。

✨ 核心功能特色
✅ 多重身份使用者系統：使用者可動態申請並擁有多種創作者身份 (老師、店家、服務提供者等)。

✅ 多店家市集 (Marketplace)：支援無數獨立店家進駐，各自管理自己的商品、課程與文章。

✅ 線上課程 (E-learning)：老師可以建立包含章節的完整線上課程並進行銷售。

✅ 實體商品 (E-commerce)：店家可以上架、管理及銷售實體商品，包含庫存追蹤。

✅ 線上預約服務 (Booking System)：服務提供者可建立服務項目，並讓顧客線上預約特定時段或區間。

✅ 通用購物車與訂單系統：採用多態關聯 (Polymorphic Association) 設計，同一個購物車和訂單系統可以無縫處理商品、課程和服務的混合結帳。

✅ 靈活的折扣碼系統：支援固定金額、百分比折扣，並可應用於全站、特定店家、單一商品或課程。

✅ 會員訂閱系統：提供分級會員方案，使用者可訂閱以獲得特殊權益。

✅ 動態網站內容管理：採用後台驅動前台 (Backend-Driven UI) 架構，可由後台動態管理網站橫幅、推薦項目 (熱門店家、精選課程、人氣創作者) 及平台公告。

✅ 完整的創作者後台 API：提供豐富的 API 端點，以支援店家、老師、服務提供者管理其資產 (商品、課程、服務、訂單、文章、員工等)。

✅ 全方位安全認證：支援本地 Email/密碼註冊登入，並整合 Google, Line, Facebook 的 OAuth 2.0 登入。

🚀 系統架構與技術棧
後端框架: Node.js + Express.js

資料庫: MySQL

ORM: Sequelize (用於優雅、安全地操作資料庫)

API 文件: Swagger (OpenAPI 3.0)

認證機制: JWT (JSON Web Tokens)

日誌系統: Pino (高效能的 JSON Logger)

程序管理 (正式環境): PM2

📁 檔案結構說明
一個清晰的檔案結構是專案可維護性的關鍵。

/
├── config/
│   ├── database.js       # Sequelize 資料庫連線設定
│   └── swagger.js        # Swagger API 文件設定
├── controllers/
│   ├── auth.controller.js  # 處理所有使用者認證 (註冊, 登入, OAuth)
│   ├── user.controller.js  # 管理使用者個人資料與角色申請
│   ├── store.controller.js # 管理店家後台功能 (資訊, 員工, 文章)
│   ├── product.controller.js# 管理商品相關邏輯
│   ├── course.controller.js # 管理課程相關邏輯
│   ├── service.controller.js# 管理可預約服務相關邏輯
│   ├── cart.controller.js  # 管理通用購物車
│   ├── order.controller.js # 處理結帳與訂單查詢
│   ├── web.controller.js   # 聚合前端網站所需的公開內容
│   └── settings.controller.js# 管理平台動態設定
├── models/
│   ├── index.js          # Sequelize 核心，載入所有模型並建立關聯
│   ├── user.model.js     # 使用者核心身份模型
│   ├── userProfile.model.js# 使用者公開資料模型
│   ├── role.model.js       # 平台角色定義模型
│   ├── store.model.js      # 店家模型
│   ├── product.model.js    # 商品模型
│   ├── course.model.js     # 課程模型
│   ├── service.model.js    # 可預約服務模型
│   ├── cart.model.js       # 購物車模型
│   ├── order.model.js      # 訂單模型
│   └── ... (所有其他關聯模型)
├── routes/
│   ├── auth.routes.js      # 認證相關 API 路由
│   ├── user.routes.js      # 使用者個人資料與角色 API 路由
│   └── ... (對應各個 Controller 的路由檔案)
├── middleware/
│   ├── authenticateToken.js# JWT 驗證中介軟體
│   └── authorization.js    # 角色權限檢查中介軟體
├── utils/
│   └── logger.js           # Pino 日誌設定
├── .env.example          # 環境變數設定檔範本
├── .env                  # 環境變數 (!! 不可提交至 Git !!)
├── server.js             # 應用程式進入點
└── README.md             # 本文件


🗃️ 資料庫結構圖 (Schema Diagram)
這張圖展示了我們整個平台複雜而有序的資料庫關聯結構。

erDiagram
    USERS {
        int id PK
        string email
        string provider
    }

    USER_PROFILES {
        int id PK
        string nickname
        string avatarUrl
        int userId FK
    }

    ROLES {
        int id PK
        string name
    }

    USER_ROLES {
        int userId PK, FK
        int roleId PK, FK
        enum status
    }

    STORES {
        int id PK
        string name
        string slug
        int ownerId FK
    }

    PRODUCTS {
        int id PK
        string name
        decimal price
        int storeId FK
    }

    COURSES {
        int id PK
        string title
        decimal price
        int instructorId FK
    }
    
    SERVICES {
        int id PK
        string title
        decimal price
        int providerId FK
    }

    BOOKINGS {
        int id PK
        datetime startTime
        int customerId FK
        int serviceId FK
    }
    
    ORDERS {
        int id PK
        decimal totalAmount
        int userId FK
    }

    ORDER_ITEMS {
        int id PK
        int quantity
        decimal priceAtPurchase
        int orderableId
        string orderableType
        int orderId FK
    }

    USERS ||--|{ USER_PROFILES : "擁有"
    USERS }o--o{ ROLES : "擁有多個"
    USERS ||--|{ STORES : "開設"
    USERS ||--o{ COURSES : "創建"
    USERS ||--o{ SERVICES : "提供"
    USERS ||--o{ ORDERS : "下訂"
    USERS ||--o{ BOOKINGS : "預約"

    STORES ||--o{ PRODUCTS : "銷售"
    
    ORDERS ||--o{ ORDER_ITEMS : "包含"
    
    SERVICES ||--o{ BOOKINGS : "被預約"


📖 API 文件
本專案使用 Swagger (OpenAPI 3.0) 自動產生互動式 API 文件。當應用程式運行後，請瀏覽以下網址：

http://localhost:3000/api-docs

授權 (Authorize)：許多 API 端點都需要 JWT 認證。請先呼叫 /api/auth/login 或 /api/auth/register 取得 token，然後點擊右上角的 "Authorize" 按鈕，在 bearerAuth 的輸入框中貼上您的 token (無需加上 "Bearer " 前綴)，即可測試所有受保護的 API。

🛠️ 安裝與部署流程
請依照以下步驟來設定、運行及部署此專案。

1. 環境準備
Node.js: 建議使用 v16 或更新版本。

MySQL: 建議使用 v8.0 或更新版本。

Git: 用於版本控制。

2. 本地開發設定 (Development)
複製專案

git clone <your-repository-url>
cd <project-name>


安裝依賴套件

npm install


設定環境變數
複製範本檔案：cp .env.example .env
打開 .env 檔案，並填入您本地的設定，至少需要填寫：

DB_HOST, DB_USER, DB_PASSWORD, DB_NAME

JWT_SECRET (請使用一個長且隨機的字串)

設定資料庫

請確保您的 MySQL 服務正在運行。

手動建立一個資料庫，名稱需與 .env 中的 DB_NAME 相同。

啟動開發伺服器

npm run dev


此指令會使用 nodemon 啟動伺服器，當您修改程式碼時，伺服器會自動重啟。
第一次啟動時，Sequelize 的 sync({ alter: true }) 功能會自動根據您的模型在資料庫中建立所有需要的資料表。

3. 正式環境部署 (Production)
將專案部署到正式伺服器需要更嚴謹的步驟，以確保穩定性與安全性。

設定環境變數

在您的正式伺服器上，同樣建立 .env 檔案。

極重要: 將 NODE_ENV 設為 production。

NODE_ENV=production


填寫連向正式資料庫的資訊，並使用一個極度安全的 JWT_SECRET。

資料庫遷移 (Migrations) - 建議做法
在正式環境中，不建議使用 sync() 來自動變更資料庫結構。應改用 Sequelize Migrations 來進行版本控制。

安裝 Sequelize CLI: npm install --save-dev sequelize-cli

建立遷移檔案: npx sequelize-cli migration:generate --name <migration-name>

執行遷移: npx sequelize-cli db:migrate

安裝 PM2 程序管理器
PM2 是一個強大的程序管理器，它能讓您的 Node.js 應用在背景運行，並在發生錯誤崩潰時自動重啟，確保服務不中斷。

# 在伺服器上全域安裝 PM2
npm install pm2 -g


啟動應用程式
使用 PM2 來啟動您的應用程式，並啟用叢集模式 (Cluster Mode) 以發揮多核心 CPU 的最大效能。

# -i 0 或 -i max 會自動根據 CPU 核心數啟動對應數量的實例
# --name 為您的應用程式命名，方便管理
pm2 start server.js --name "creator-platform-api" -i 0


設定開機自啟動
讓伺服器重開機後，PM2 能自動啟動您的應用程式。

# 產生開機腳本，並依照提示執行指令
pm2 startup

# 儲存當前運行的應用程式列表
pm2 save


常用 PM2 指令

pm2 list: 查看所有應用程式狀態。

pm2 logs: 查看即時日誌。

pm2 monit: 打開監控儀表板。

pm2 restart <app_name>: 重啟應用程式。

pm2 stop <app_name>: 停止應用程式。

設定反向代理 (Nginx) - 強烈建議
在您的 Node.js 應用程式前架設一個像 Nginx 這樣的網頁伺服器作為反向代理，可以帶來許多好處：

SSL/TLS 終止: 處理 HTTPS 加密，讓您的 API 更安全。

負載平衡: 將請求分發到 PM2 啟動的多個實例。

靜態檔案服務: 更高效地提供圖片等靜態資源。

📄 環境變數說明 (.env)
| 變數 | 說明 | 範例 |
| NODE_ENV | 應用程式運行環境 | development / production |
| API_PORT | 後端伺服器運行的埠號 | 3000 |
| LOG_LEVEL | 日誌輸出的詳細程度 | info / debug / warn |
| DB_HOST | 資料庫主機位址 | localhost |
| DB_USER | 資料庫使用者名稱 | root |
| DB_PASSWORD | 資料庫密碼 | your_password |
| DB_NAME | 資料庫名稱 | creator_platform_db |
| JWT_SECRET | JWT 簽發密鑰 (必須是長且隨機的字串) | a_very_long_and_random_secret_string |
