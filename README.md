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
