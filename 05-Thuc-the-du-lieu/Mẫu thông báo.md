---
ten: "Mẫu thông báo"
loai: thực thể
bang_db: notification_template
module_chu: BC
trang_thai: Nháp
tags:
  - thuc-the
---

# Mẫu thông báo

> Bảng: `notification_template` · Module chủ: BC

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `code` | text | — | ✓ | Mã mẫu, duy nhất |
| `name` | text | — | ✓ | Tên mẫu |
| `channel` | text | — | ✓ | enum: `zalo` / `sms` / `email` |
| `subject` | text | — | – | Tiêu đề (email) |
| `body_template` | text | — | ✓ | Nội dung mẫu, có chỗ chèn biến |

## Ràng buộc & chỉ mục

- Unique: `code`
