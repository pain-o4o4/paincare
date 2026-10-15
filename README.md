# 🏥 BookingCare - Nền Tảng Đặt Lịch Khám Bệnh & Quản Lý Y Tế Toàn Diện

> **BookingCare** là hệ thống y tế trực tuyến toàn diện (Fullstack Web Application), kết nối Bệnh nhân, Bác sĩ và Cơ sở y tế. Dự án tích hợp các công nghệ hiện đại: Trợ lý AI tư vấn y tế với RAG (Retrieval-Augmented Generation), Chat Real-time qua WebSocket, Cổng thanh toán trực tuyến PayOS/VNPay, Hệ thống mẫu Email động, và quy trình CI/CD tự động hóa lên AWS EC2 & Vercel.

---

## 📑 Mục Lục
- [Tổng Quan Kiến Trúc](#-tổng-quan-kiến-trúc)
- [Tính Năng Nổi Bật](#-tính-năng-nổi-bật)
- [Công Nghệ Sử Dụng (Tech Stack)](#-công-nghệ-sử-dụng-tech-stack)
- [Cấu Trúc Thư Mục](#-cấu-trúc-thư-mục)
- [Cài Đặt & Khởi Chạy Dự Án](#-cài-đặt--khởi-chạy-dự-án)
  - [1. Yêu Cầu Môi Trường](#1-yêu-cầu-môi-trường)
  - [2. Cấu Hình Biến Môi Trường (.env)](#2-cấu-hình-biến-môi-trường-env)
  - [3. Chạy Môi Trường Cục Bộ (Local)](#3-chạy-môi-trường-cục-bộ-local)
  - [4. Chạy Bằng Docker Compose](#4-chạy-bằng-docker-compose)
- [Phân Quyền & Bảo Mật](#-phân-quyền--bảo-mật)
- [Tích Hợp AI & RAG](#-tích-hợp-ai--rag)
- [Quy Trình CI/CD & Triển Khai](#-quy-trình-cicd--triển-khai)

---

## 🏛 Tổng Quan Kiến Trúc

Hệ thống được xây dựng theo mô hình Client - Server tách biệt:
- **Client (Frontend):** Ứng dụng Single Page Application (SPA) xây dựng bằng **ReactJS 18**, quản lý trạng thái bằng **Redux**, hỗ trợ đa ngôn ngữ (Tiếng Việt / English).
- **Server (Backend):** RESTful API xây dựng bằng **Node.js (ExpressJS)** theo chuẩn ES6 (Babel), kết nối cơ sở dữ liệu quan hệ **MySQL 8** thông qua **Sequelize ORM**.
- **Real-time Server:** **Socket.IO** đồng bộ sự kiện và tin nhắn tức thì.
- **AI & Vector Engine:** **Google Generative AI (Gemini)** kết hợp **Pinecone Vector Database** tạo luồng RAG tìm kiếm thông minh và hỏi đáp chuyên sâu.

---

## ✨ Tính Năng Nổi Bật

### 1. Phân Hệ Bệnh Nhân (Patient)
- 🔍 **Tìm Kiếm Đa Năng:** Tìm kiếm bác sĩ, phòng khám, chuyên khoa với bộ lọc nâng cao và lưu lại lịch sử tìm kiếm.
- 📅 **Đặt Lịch Khám Bệnh:** Xem khung giờ khám trực quan theo từng ngày, chọn bác sĩ và đặt lịch khám online tiện lợi.
- 💳 **Thanh Toán Trực Tuyến:** Tích hợp cổng thanh toán tự động qua **PayOS / VNPay** kèm xác thực webhook bảo mật.
- 💬 **Tư Vấn Y Tế Thông Minh (AI Chatbot):** Hỏi đáp triệu chứng sức khỏe, tư vấn chọn chuyên khoa nhờ Gemini AI & cơ sở tri thức Pinecone RAG.
- 💬 **Live Chat 1-1:** Trò chuyện trực tiếp với Bác sĩ hoặc phòng khám theo thời gian thực (hỗ trợ tin nhắn hình ảnh, icon phản hồi).
- 📜 **Lịch Sử Đặt Khám & Hồ Sơ Cá Nhân:** Theo dõi trạng thái lịch khám (chờ duyệt, đã khám, đã hủy), quản lý thông tin sức khỏe.

### 2. Phân Hệ Bác Sĩ (Doctor)
- ⏰ **Quản Lý Kế Hoạch Khám:** Tạo và chỉnh sửa khung giờ khám bệnh (Bulk Create Schedule) theo ngày.
- 📋 **Quản Lý Bệnh Nhân Đặt Khám:** Xem danh sách bệnh nhân theo ngày khám, cập nhật trạng thái lịch hẹn (Xác nhận, Đã khám, Hủy).
- ✉️ **Gửi Hóa Đơn & Đơn Thuốc:** Gửi email kèm đơn thuốc/hóa đơn khám bệnh cho bệnh nhân.
- 💬 **Tương Tác Bệnh Nhân:** Chat tư vấn trực tiếp, sử dụng các câu trả lời nhanh (Quick Replies) để tối ưu thời gian.

### 3. Phân Hệ Quản Trị Viên (Admin)
- 👥 **Quản Lý Người Dùng:** Phân quyền theo vai trò (Admin, Doctor, Patient), tạo/sửa/xóa tài khoản an toàn (hỗ trợ Soft Delete).
- 🏥 **Quản Lý Danh Mục:** Quản lý Chuyên khoa (Specialty), Phòng khám (Clinic), Bài viết Cẩm nang (Handbook).
- 📧 **Admin Communication Hub:**
  - **Quản lý Mẫu Email (Email Templates):** Soạn thảo mẫu email gửi tự động linh hoạt với biến động dạng `{{patientName}}`, `{{time}}`, `{{doctorName}}`...
  - **Quản lý Trả Lời Nhanh Toàn Cục (Global Quick Replies):** Cấu hình các mẫu câu chat tiện ích dùng chung cho các bác sĩ.
- 📊 **Thống Kê Hệ Thống:** Báo cáo số liệu lịch hẹn, doanh thu, tăng trưởng người dùng.
- 🔄 **Đồng Bộ RAG (Sync RAG):** Cập nhật dữ liệu từ MySQL lên Pinecone Vector Database để AI nắm bắt thông tin mới nhất.

---

## 🛠 Công Nghệ Sử Dụng (Tech Stack)

| Lớp | Công Nghệ |
| :--- | :--- |
| **Frontend** | React 18, Redux, Redux Thunk, Redux Persist, React Router v6, Bootstrap 5, SCSS, Axios, React-Intl |
| **Backend** | Node.js, Express.js, Babel (ES6+), Sequelize ORM, Socket.IO, Nodemailer, Bcryptjs, JWT |
| **Cơ Sở Dữ Liệu** | MySQL 8.0, Pinecone Vector Database |
| **AI / GenAI** | Google Generative AI (Gemini), Embeddings, RAG Architecture |
| **Cổng Thanh Toán** | PayOS SDK, VNPay |
| **Lưu Trữ Ảnh** | Cloudinary API |
| **DevOps / CI/CD** | Docker, Docker Compose, GitHub Actions, AWS EC2, Vercel |

---

## 📂 Cấu Trúc Thư Mục

```text
fullstack/
├── .github/
│   └── workflows/
│       └── deploy.yml           # CI/CD GitHub Actions: Test Babel & Deploy AWS EC2
├── docker-compose.yml           # Khởi chạy cụm MySQL + NodeJS Backend
├── NodeJS/                      # Source code Backend
│   ├── src/
│   │   ├── config/              # Cấu hình CSDL, Phân quyền (permissions.js), Pinecone, PayOS
│   │   ├── controller/          # Điều hướng xử lý Request (User, Doctor, Admin, Chat, RAG...)
│   │   ├── middleware/          # JWT Auth, Rate Limiter, Socket Sync, Search Validator
│   │   ├── route/               # Định nghĩa các API endpoints (web.js)
│   │   ├── services/            # Nghiệp vụ cốt lõi (User, Doctor, Admin, Email, Chatbot, RAG...)
│   │   ├── socket/              # Xử lý sự kiện WebSocket (Socket.IO)
│   │   └── utils/               # Tiện ích bổ trợ (Cache, Template placehoder, ImageUtils...)
│   ├── models/                  # Định nghĩa Sequelize Models & Associations
│   ├── migrations/              # Lịch sử cập nhật cấu trúc Database
│   ├── Dockerfile               # Dockerfile đóng gói NodeJS Backend
│   └── package.json
└── ReactJS/                     # Source code Frontend
    ├── public/
    ├── src/
    │   ├── auth/                # Cấu hình Axios Instance & Interceptors
    │   ├── components/          # Các Component giao diện tái sử dụng
    │   ├── containers/          # Các trang nghiệp vụ (Patient, Doctor, System Admin, Auth...)
    │   ├── routes/              # Điều hướng Frontend (App, System, Doctor...)
    │   ├── services/            # Hàm gọi API Backend (axios)
    │   ├── store/               # Redux Reducers & Actions
    │   └── translations/        # Từ điển đa ngôn ngữ (vi.json, en.json)
    └── package.json
```

---

## 🚀 Cài Đặt & Khởi Chạy Dự Án

### 1. Yêu Cầu Môi Trường
- **Node.js:** Phiên bản 18.x - 22.x LTS
- **Package Manager:** `npm` hoặc `yarn`
- **Cơ sở dữ liệu:** MySQL 8.0
- **Docker & Docker Compose** (Tùy chọn nếu muốn chạy container)

---

### 2. Cấu Hình Biến Môi Trường (.env)

#### Backend (`NodeJS/.env`):
Tạo file `.env` trong thư mục `NodeJS/` dựa theo mẫu sau:
```env
PORT=2004
NODE_ENV=development
URL_REACT=http://localhost:3000

# Database Cấu hình kết nối MySQL
DB_HOST=127.0.0.1
DB_PORT=3306
DB_NAME=bookingcare
DB_USER=root
DB_PASSWORD=your_mysql_password

# Authentication (JWT)
JWT_SECRET=your_jwt_access_secret_key
JWT_EXPIRES_IN=15m
JWT_REFRESH_SECRET=your_jwt_refresh_secret_key
JWT_REFRESH_EXPIRES_IN=7d

# Gửi Email (Gmail App Password)
EMAIL_APP=your_email@gmail.com
EMAIL_APP_PASSWORD=your_gmail_app_password

# Cổng Thanh Toán PayOS
PAYOS_CLIENT_ID=your_payos_client_id
PAYOS_API_KEY=your_payos_api_key
PAYOS_CHECKSUM_KEY=your_payos_checksum_key

# Lưu Trữ Ảnh Cloudinary
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_api_key
CLOUDINARY_API_SECRET=your_api_secret

# Trí Tuệ Nhân Tạo (Gemini & Pinecone)
GOOGLE_API_KEY=your_google_gemini_api_key
GOOGLE_MODEL_NAME=gemini-1.5-flash
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX=your_pinecone_index_name
```

#### Frontend (`ReactJS/.env`):
Tạo file `.env` trong thư mục `ReactJS/`:
```env
PORT=3000
NODE_ENV=development
REACT_APP_BACKEND_URL=http://localhost:2004
REACT_APP_ROUTER_BASE_NAME=
```

---

### 3. Chạy Môi Trường Cục Bộ (Local)

#### Bước 1: Cài đặt và di chuyển cấu trúc CSDL (Backend)
```bash
cd NodeJS

# Cài đặt các gói phụ thuộc
npm install

# Chạy Migration để khởi tạo bảng dữ liệu
npx sequelize-cli db:migrate

# Khởi chạy server Backend ở chế độ phát triển
npm run dev
```
*Backend sẽ lắng nghe tại:* `http://localhost:2004`

#### Bước 2: Khởi chạy Frontend
Mở một terminal mới:
```bash
cd ReactJS

# Cài đặt các gói phụ thuộc
npm install

# Khởi động ứng dụng React
npm start
```
*Giao diện người dùng sẽ mở tại:* `http://localhost:3000`

---

### 4. Chạy Bằng Docker Compose

Nếu bạn muốn khởi chạy nhanh toàn bộ Backend và MySQL chỉ bằng một lệnh:
```bash
# Tại thư mục gốc của dự án
docker compose up -d --build
```
- Lệnh sẽ tự động khởi động MySQL container, kiểm tra Healthcheck và sau đó khởi động Backend.

---

## 🔒 Phân Quyền & Bảo Mật

Hệ thống áp dụng cơ chế xác thực và phân quyền phân lớp:
1. **Xác thực:** Sử dụng cặp khóa `Access Token` (gửi qua HTTP Header `Bearer`) và `Refresh Token` (lưu trữ an toàn trong `HttpOnly Cookie`, chống XSS).
2. **Kiểm soát truy cập (RBAC):** Middleware `authMiddleware.js` kết hợp danh sách quyền tại `src/config/permissions.js`:
   - **Public:** Xem danh sách bác sĩ, chuyên khoa, phòng khám, tìm kiếm, đăng ký/đăng nhập.
   - **Patient (R3):** Đặt lịch, xem lịch sử khám, thanh toán, tương tác chat.
   - **Doctor (R2):** Quản lý lịch khám, tiếp nhận bệnh nhân, gửi đơn thuốc/hóa đơn.
   - **Admin (R1):** Toàn quyền kiểm soát hệ thống, quản lý tài khoản, danh mục, mẫu email.
3. **Bảo vệ hệ thống:** 
   - **Rate Limiting:** Chống spam truy vấn tìm kiếm và DDOS nhẹ.
   - **Soft Delete:** Tránh mất mát dữ liệu quan trọng khi thao tác xóa.

---

## 🤖 Tích Hợp AI & RAG

- **Retrieval-Augmented Generation (RAG):**
  - Dữ liệu về bác sĩ, chuyên khoa, bảng giá dịch vụ được vector hóa (Embeddings) và lưu trữ trên **Pinecone**.
  - Khi bệnh nhân đặt câu hỏi triệu chứng trên Chatbot, hệ thống tìm kiếm ngữ nghĩa các văn bản y tế liên quan trong Pinecone và gửi kèm vào ngữ cảnh (Context) cho **Google Gemini**.
  - Trợ lý AI đưa ra câu trả lời chuẩn xác, khách quan và gợi ý đúng chuyên khoa/bác sĩ phù hợp trong hệ thống.

---

## 🚢 Quy Trình CI/CD & Triển Khai

- **Backend (AWS EC2):** 
  - Tự động kích hoạt khi có commit đẩy lên nhánh `main`.
  - GitHub Actions chạy bước **Babel Build Test** để đảm bảo không lỗi cú pháp.
  - Tự động SSH vào máy chủ AWS EC2, đồng bộ code mới và cập nhật container qua `docker compose up -d --build`.
- **Frontend (Vercel):** 
  - Tự động triển khai nhanh chóng trên nền tảng Vercel với CDN toàn cầu.

---

## 👨‍💻 Tác Giả & Giấy Phép
- **Tác giả:** Vương Thế Anh (pain-o4o4)
- **Email:** `64anhsden@gmail.com`
- **Mã nguồn:** [GitHub - pain-o4o4/fullstack](https://github.com/pain-o4o4/fullstack)
