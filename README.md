# [Thực Hành] Hướng dẫn cài đặt SASS bằng npm & SASS CLI

## 1️⃣ Mục tiêu bài thực hành
- Kiểm tra và đảm bảo môi trường Node.js và npm đã được cài đặt trên máy tính.
- Cài đặt và sử dụng SASS (Dart SASS) để chuyển đổi tệp SCSS sang CSS.
- Thành thạo việc sử dụng SASS CLI và npm scripts để quản lý, biên dịch tệp đơn lẻ và nhiều tệp theo thư mục.
- Nắm vững cơ chế tự động theo dõi `--watch` và khắc phục sự cố phân quyền trên Windows.

---

## 2️⃣ Nội dung và Kết quả thực hiện từng bước

### 🔹 Bước 1, 2 & 3: Kiểm tra môi trường Node.js và npm
Mở Terminal / PowerShell và chạy các lệnh kiểm tra:
```bash
# Kiểm tra phiên bản Node.js
node -v
# Kết quả thực tế trên máy: v24.19.0

# Kiểm tra phiên bản npm
npm -v
# Kết quả thực tế trên máy: 11.17.0
```
> **Ghi chú:** Máy tính đã cài đặt đầy đủ phiên bản Node.js và npm mới nhất, sẵn sàng để cài đặt các package toàn cục.

---

### 🔹 Bước 4: Cài đặt SASS bằng npm
Chạy lệnh cài đặt SASS ở chế độ toàn cục (global) bằng npm:
```bash
npm install -g sass
```
Sau khi cài đặt hoàn tất, kiểm tra phiên bản SASS:
```bash
sass --version
# Kết quả thực tế: 1.105.1 compiled with dart2js 3.13.5
```
> **Ý nghĩa:** Cờ `-g` giúp lệnh `sass` có thể được gọi từ bất kỳ thư mục nào trong Command Prompt hoặc PowerShell.

---

### 🔹 Bước 5: Cài đặt SASS CLI mà không cần npm (Phương pháp dự phòng)
Ngoài npm, có thể tải trực tiếp file thực thi độc lập:
1. Truy cập [SASS GitHub Releases](https://github.com/sass/dart-sass/releases).
2. Tải phiên bản dành cho Windows (`dart-sass-*-windows-x64.zip`).
3. Giải nén vào thư mục cố định (ví dụ `C:\Program Files\SASS\`) và thêm đường dẫn vào biến môi trường `PATH`.
4. Kiểm tra lại bằng lệnh: `sass --version`.

---

### 🔹 Bước 6: Chuyển đổi từ tệp SCSS sang CSS bằng SASS CLI (Đơn tệp)
Tạo file [styles.scss](file:///d:/BaiTapGithub/styles.scss) trong thư mục dự án và tiến hành biên dịch:
```bash
sass styles.scss styles.css
```
- **Kết quả:** SASS CLI tự động biên dịch và tạo ra 2 tệp đầu ra:
  - [styles.css](file:///d:/BaiTapGithub/styles.css): Chứa toàn bộ mã CSS đã chuyển đổi.
  - `styles.css.map`: Source map phục vụ việc kiểm tra và gỡ lỗi trên trình duyệt.

---

### 🔹 Bước 7: Kích hoạt chế độ theo dõi (Watch mode)
Để SASS tự động biên dịch mỗi khi lưu tệp SCSS:
```bash
sass styles.scss styles.css --watch
```
- Khi chỉnh sửa [styles.scss](file:///d:/BaiTapGithub/styles.scss), terminal sẽ tự động hiển thị:
  `Sass is watching for changes. Press Ctrl-C to stop.`

---

### 🔹 Bước 8: Biên dịch nhiều tệp SCSS cùng lúc theo thư mục (`scss/:css/`)
Xây dựng cấu trúc thư mục chứa nhiều file SCSS:
- Thư mục đầu vào `scss/` chứa:
  - `_variables.scss`, `_mixins.scss`, `_base.scss`, `_components.scss` (các file partial).
  - `main.scss` (tổng hợp các partials).
  - `page.scss` (file trang tài liệu độc lập).
- Thực hiện biên dịch toàn bộ thư mục:
```bash
sass scss/:css/
# Hoặc tự động theo dõi toàn bộ thư mục:
sass scss/:css/ --watch
```
- **Kết quả:** Thư mục `css/` tự động được tạo và chứa:
  - [css/main.css](file:///d:/BaiTapGithub/css/main.css) và `main.css.map`
  - [css/page.css](file:///d:/BaiTapGithub/css/page.css) và `page.css.map`

---

### 🔹 Bước 9: Xử lý lỗi "Set-ExecutionPolicy" trên Windows
Nếu gặp lỗi `running scripts is disabled on this system` khi chạy PowerShell, mở PowerShell với quyền Administrator và chạy lệnh:
```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
# Hoặc:
Set-ExecutionPolicy Unrestricted
```

---

## 📁 Cấu trúc thư mục dự án

```
D:\BaiTapGithub\
├── scss/                     # Thư mục đầu vào chứa nhiều file SCSS (Mục 8)
│   ├── _variables.scss       # Khai báo biến
│   ├── _mixins.scss          # Khai báo mixin
│   ├── _base.scss            # Định dạng reset & typography
│   ├── _components.scss      # Khối terminal, banner, nút bấm
│   ├── main.scss             # File SCSS chính -> css/main.css
│   └── page.scss             # File SCSS phụ -> css/page.css
├── css/                      # Thư mục đầu ra chứa CSS được biên dịch tự động
│   ├── main.css
│   ├── main.css.map
│   ├── page.css
│   └── page.css.map
├── styles.scss               # File SCSS đơn lẻ kiểm thử biên dịch đơn tệp (Mục 6 & 7)
├── styles.css                # CSS đầu ra từ styles.scss
├── styles.css.map            # Source map của styles.css
├── index.html                # Trang giao diện trực quan hiển thị đầy đủ 9 bước thực hành
├── package.json              # Khai báo npm scripts quản lý SASS
├── .gitignore
└── README.md                 # Báo cáo kết quả bài tập chi tiết
```

---

## 🛠 Lệnh NPM Scripts hỗ trợ sẵn
Trong [package.json](file:///d:/BaiTapGithub/package.json), các script sau đã được cấu hình sẵn:
```bash
npm run sass:version         # Kiểm tra phiên bản SASS
npm run sass:single          # Biên dịch đơn tệp: sass styles.scss styles.css
npm run sass:single-watch    # Watch đơn tệp: sass styles.scss styles.css --watch
npm run sass:multi           # Biên dịch thư mục: sass scss/:css/
npm run sass:multi-watch     # Watch thư mục: sass scss/:css/ --watch
npm run build                # Biên dịch tất cả (cả đơn tệp và thư mục)
```

---

## 🎯 Tổng kết
✔ Đã kiểm tra môi trường Node.js (v24.19.0) và npm (11.17.0).  
✔ Đã cài đặt thành công SASS CLI (v1.105.1).  
✔ Đã thực hành biên dịch đơn tệp `styles.scss -> styles.css`.  
✔ Đã thực hành cấu trúc thư mục đầu vào/đầu ra `scss/:css/` biên dịch nhiều tệp thành công.  
✔ Đã hoàn thành trang demo trực quan [index.html](file:///d:/BaiTapGithub/index.html) và đẩy toàn bộ mã nguồn lên GitHub.
