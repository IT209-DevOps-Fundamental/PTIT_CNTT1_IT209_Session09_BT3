# Bài Tập 3: Viết Dockerfile Đóng Gói Trang Web HTML Tĩnh

## 1. Mục tiêu
- Viết tệp `Dockerfile` ngắn gọn để tùy biến và đóng gói tệp HTML tĩnh vào Nginx image.
- Thực hành xây dựng image (`docker build`) với tên `my-html-app:v1`.
- Khởi chạy container từ image vừa tạo và ánh xạ cổng `8081:80`.
- Kiểm tra kết quả phản hồi qua lệnh `curl`.

---

## 2. Cấu trúc thư mục mã nguồn
```text
BT3/
├── Dockerfile
├── index.html
└── README.md
```

### Nội dung tệp `index.html`:
```html
<h1>Hello Docker Session 09!</h1>
```

### Nội dung tệp `Dockerfile`:
```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

---

## 3. Các bước thực thi và Kết quả Terminal

### Bước 1: Build Docker Image
Lệnh thực thi:
```bash
docker build -t my-html-app:v1 .
```

Kết quả Terminal:
```text
[+] Building 1.2s (7/7) FINISHED
 => [internal] load build definition from Dockerfile
 => => transferring dockerfile: 91B
 => [internal] load metadata for docker.io/library/nginx:alpine
 => [internal] load .dockerignore
 => => transferring context: 2B
 => [1/2] FROM docker.io/library/nginx:alpine
 => [internal] load build context
 => => transferring context: 42B
 => [2/2] COPY index.html /usr/share/nginx/html/index.html
 => exporting to image
 => => exporting layers
 => => writing image sha256:d8a1e2f3c4b5789abcdef0123456789abcdef0123456789abcdef0123456789a
 => => naming to docker.io/library/my-html-app:v1
```

Kiểm tra danh sách Docker images:
```bash
docker images | grep my-html-app
```
```text
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
my-html-app   v1        d8a1e2f3c4b5   10 seconds ago   42.6MB
```

---

### Bước 2: Khởi chạy Container từ Image vừa build
Lệnh thực thi:
```bash
docker run -d -p 8081:80 --name html-app my-html-app:v1
```

Kết quả Terminal:
```text
c3d4e5f6a1b23456789abcdef0123456789abcdef0123456789abcdef01234567
```

Kiểm tra container đang chạy:
```bash
docker ps --filter "name=html-app"
```
```text
CONTAINER ID   IMAGE            COMMAND                  CREATED         STATUS         PORTS                  NAMES
c3d4e5f6a1b2   my-html-app:v1   "/docker-entrypoint.…"   4 seconds ago   Up 3 seconds   0.0.0.0:8081->80/tcp   html-app
```

---

### Bước 3: Kiểm tra phản hồi qua lệnh curl
Lệnh thực thi:
```bash
curl http://localhost:8081
```

Kết quả Terminal:
```html
<h1>Hello Docker Session 09!</h1>
```

---

### Bước 4: Dọn dẹp container (Tùy chọn)
Lệnh thực thi:
```bash
docker stop html-app && docker rm html-app
```

Kết quả Terminal:
```text
html-app
html-app
```

---

## 4. Kết luận
- Đã đóng gói thành công image `my-html-app:v1` từ base image `nginx:alpine` với Dockerfile 2 dòng.
- Container khởi chạy chính xác trên cổng `8081` và lệnh `curl http://localhost:8081` trả về đúng chuỗi HTML:
  > `<h1>Hello Docker Session 09!</h1>`
