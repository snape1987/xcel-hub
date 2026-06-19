# Triển khai hub.xcel.vn qua FTP

Site tĩnh được deploy tự động lên FTP server bằng GitHub Actions
(`.github/workflows/deploy-ftp.yml`).

## 1. Thêm GitHub Secrets (làm 1 lần)

Vào **Repo → Settings → Secrets and variables → Actions → New repository secret**
và thêm 3 secret sau:

| Secret name    | Giá trị            |
|----------------|--------------------|
| `FTP_SERVER`   | `171.244.29.139`   |
| `FTP_USERNAME` | `xcel`             |
| `FTP_PASSWORD` | `Xcel@123`         |

> Cổng (21), giao thức (FTP), chế độ passive và thư mục đích (`./`) đã được
> cấu hình sẵn trong workflow nên không cần thêm secret cho các mục này.

## 2. Cách deploy

- **Tự động:** mỗi lần push/merge vào nhánh `master`.
- **Thủ công:** vào tab **Actions → Deploy to FTP (hub.xcel.vn) → Run workflow**.

## 3. File được đẩy lên

Toàn bộ nội dung web ở repo, trừ các file CI/nội bộ (xem `exclude` trong workflow):

```
hub-landing.html
hub-quiz.html
ket-qua/khoa-hoc-ai-tai-chinh-ke-toan-2026-06-16.html
```

## Lưu ý

- Nếu web root thực tế không phải thư mục đăng nhập của user `xcel`, sửa
  `server-dir` trong workflow (vd: `/public_html/` hoặc `/hub.xcel.vn/`).
- Action dùng [SamKirkland/FTP-Deploy-Action](https://github.com/SamKirkland/FTP-Deploy-Action),
  chỉ upload file thay đổi (theo `.ftp-deploy-sync-state.json` lưu trên server).
