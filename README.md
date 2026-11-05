# Bài 6 : Ngăn chặn rò rỉ thông tin bí mật trên GitHub
## 2. Yêu cầu chức năng

- Xây dựng Pre-push Checklist để quét sạch các API Keys bị hardcode.

- Thiết lập cấu trúc tệp **.env.example** chuyên nghiệp cho các môi trường Dev, Test, và Production.

- Hướng dẫn team kích hoạt 2FA bắt buộc cho mọi tài khoản tham gia repository.

## 3. Yêu cầu đầu ra

- Một repository mẫu trên GitHub chứa cấu trúc file an toàn và bản báo cáo đánh giá rủi ro nếu thiếu tệp .gitignore.
 ```
repo/
├── .gitignore            # chặn .env, *.pem, node_modules...
├── .env.example
├── .env.development.example
├── .env.test.example
├── .env.production.example
├── PRE_PUSH_CHECKLIST.md
├── 2FA_GUIDE.md
├── RISK_ASSESSMENT.md    # báo cáo rủi ro
└── README.md
```
# Pre-push Checklist (đẩy mã nguồn lên GitHub Public)

## Bước 1: Kiểm tra mã nguồn, xác định thông tin nhạy cảm
- [ ] Liệt kê loại bí mật của dự án: tài khoản DB, mật khẩu/key API, khóa thanh toán, SMTP, JWT secret, file `*.pem`
- [ ] Quét key hardcode:
      `grep -rInE "api[_-]?key|secret|token|password|AKIA|sk_live|BEGIN (RSA|PRIVATE)" --exclude-dir={node_modules,.git} .`
- [ ] Chạy công cụ quét cả lịch sử: `gitleaks detect --source . --log-opts="--all" --verbose`
- [ ] Rà soát file cấu hình (`config.json`, `application.properties`, `docker-compose.yml`, dump DB `.sql`)

## Bước 2: Loại bỏ thông tin
- [ ] Thay giá trị thật bằng biến môi trường (`process.env.X`)
- [ ] File cấu hình chứa bí mật: đổi thành `*.example` với giá trị giả, thêm bản thật vào `.gitignore`
- [ ] `git status` không có `.env`, `*.pem`, `*.key`
- [ ] Bí mật từng nằm trong lịch sử: **revoke key cũ, tạo key mới**, rồi làm sạch lịch sử bằng `git filter-repo`
- [ ] Không đưa dữ liệu khách hàng thật (email, SĐT, đơn hàng) vào dữ liệu mẫu

## Bước 3: Đẩy lên GitHub Public
- [ ] Nên tạo repo **Private** trước, kiểm tra xong mới chuyển Public
- [ ] Bật Secret scanning và Push protection (Settings > Code security)
- [ ] Mọi thành viên đã bật 2FA, nhánh `main` có Branch protection
- [ ] Commit sạch, rồi `git push`

## Nếu lỡ push bí mật
1. Revoke và tạo key mới ngay (xóa commit không đủ vì key đã lộ).
2. Làm sạch lịch sử bằng `git filter-repo` hoặc BFG, rồi force-push.
3. Kiểm tra log truy cập của dịch vụ liên quan.
