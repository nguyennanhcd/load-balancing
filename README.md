<h1 align="center">🖧 Hướng Dẫn Thiết Lập Load Balancer Với Nginx Trên WSL</h1>

<p align="center">
  <img src="https://img.shields.io/badge/WSL-Ubuntu_20.04-black?style=for-the-badge&logo=ubuntu&logoColor=white" />
  <img src="https://img.shields.io/badge/Nginx-1.18+-green?style=for-the-badge&logo=nginx&logoColor=white" />
  <img src="https://img.shields.io/badge/Platform-Windows_10+_or_11-blue?style=for-the-badge&logo=windows&logoColor=white" />
</p>

---

## 🚀 Giới Thiệu

Cân bằng tải là một phần quan trọng giúp phân phối lưu lượng truy cập đến nhiều server backend, cải thiện hiệu năng và độ tin cậy của hệ thống.  
Hướng dẫn này giúp bạn thiết lập nhanh Load Balancer sử dụng **Nginx trên WSL** (Ubuntu).

---

## ⚙️ Yêu Cầu Hệ Thống

- ✅ Windows 10 version 2004+ hoặc Windows 11
- ✅ Đã bật WSL
- ✅ Cài Ubuntu từ Microsoft Store
- ✅ Kết nối internet
- ✅ Có 3 server backend đang chạy tại các cổng: `3001`, `3002`, `3003`

---

## 🛠️ Cài Đặt Nginx Trên WSL

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx -y
sudo service nginx start
```

Truy cập trình duyệt tại `http://localhost` để kiểm tra.

---

## 🧩 Khởi Động Các Server

### 📦 Backend Servers

Chạy lệnh sau trong 3 terminal khác nhau hoặc dùng script riêng:

```bash
PORT=3001 yarn start
PORT=3002 yarn start
PORT=3003 yarn start
```

```bash
set PORT=3001 yarn start
```

> ⚠️ Đảm bảo các backend dùng cổng tương ứng và trả về nội dung phản hồi khác nhau để dễ kiểm tra load balancing.

---

### 💻 Frontend Server

Đi vào thư mục frontend và chạy:

```bash
yarn dev
```

> Frontend nên gọi API từ `http://localhost` (được proxy bởi Nginx).

---

## 📝 Cấu Hình Nginx Làm Load Balancer

```bash
sudo nano /etc/nginx/nginx.conf
```

Trong block `http {}` thêm:

```nginx
upstream backend {
    server 172.17.32.1:3001 ( địa chỉ IP của máy Windows);
    server 172.17.32.1:3002 ( địa chỉ IP của máy Windows);
    server 172.17.32.1:3003 ( địa chỉ IP của máy Windows);
}

server {
    listen 80;

    location / {
        proxy_pass http://backend;
    }
}
```

Lưu lại và reload nginx:

```bash
sudo service nginx reload
```

---

## 🧪 Kiểm Tra Load Balancer

```bash
curl http://localhost
```

Bạn sẽ thấy kết quả phản hồi từ nhiều backend khác nhau luân phiên.

---

## 💡 Mẹo Nâng Cao

- Có thể thay đổi thuật toán load balancing:
  - `least_conn;` - ít kết nối nhất
  - `ip_hash;` - phân phối theo IP người dùng
- Kết hợp với Docker hoặc PM2 để chạy server ổn định hơn

---

<p align="center">
  <img src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=22&pause=1000&center=true&width=435&lines=Happy+Coding+!+🚀" />
</p>

<!--END_SECTION:nginx-wsl-loadbalancer-->
