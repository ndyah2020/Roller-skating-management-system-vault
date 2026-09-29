---
ten: "Phụ huynh"
loai: thực thể
bang_db: guardian
module_chu: HV
trang_thai: Nháp
tags:
  - thuc-the
---

# Phụ huynh

> Bảng: `guardian` · Module chủ: HV

Một dòng = một phụ huynh/người giám hộ. Người đăng nhập thay cho con qua [[Tài khoản đăng nhập]] (`user.guardian_id`).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `full_name` | text | — | ✓ | Họ tên |
| `phone` | text | — | ✓ | Số điện thoại, duy nhất — dùng để đăng nhập |
| `zalo` | text | — | – | Số Zalo nhận thông báo, nếu khác `phone` |
| `email` | text | — | – | Email liên hệ |
| `address` | text | — | – | Địa chỉ |
| `occupation` | text | — | – | Nghề nghiệp *(tuỳ chọn, có thể bỏ nếu không dùng)* |

## Ràng buộc & chỉ mục

- Unique: `phone`

## Ghi chú

- Quan hệ nhiều-nhiều với học viên nằm ở [[Quan hệ học viên – phụ huynh]].
