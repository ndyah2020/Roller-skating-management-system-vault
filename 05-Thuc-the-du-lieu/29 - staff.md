---
ten: "Hồ sơ HLV"
loai: thực thể
bang_db: staff
module_chu: NS
so_thu_tu: 29
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Hồ sơ HLV"
---

# staff

> #29 · Hồ sơ HLV · Module chủ: NS

Một dòng = một huấn luyện viên. Gắn với tài khoản đăng nhập qua `user.staff_id`.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `full_name` | text | — | ✓ | Họ tên |
| `birth_date` | date | — | – | Ngày sinh |
| `gender` | text | — | – | Giới tính |
| `id_card_no` | text | — | – | Số CCCD, duy nhất nếu có |
| `phone` | text | — | ✓ | Số điện thoại, duy nhất |
| `email` | text | — | – | Email |
| `address` | text | — | – | Địa chỉ |
| `emergency_contact` | text | — | – | Người liên hệ khẩn cấp |
| `employment_type` | text | — | ✓ | enum: `full_time` / `part_time` |
| `start_date` | date | — | ✓ | Ngày bắt đầu làm |
| `status` | text | — | ✓ | enum: `active` / `on_leave` / `terminated` |

## Ràng buộc & chỉ mục

- Unique: `phone`; `id_card_no` (khi khác NULL)

## Quy tắc nghiệp vụ áp dụng

- [[QT-05 - Tài sản và hàng hoá]] — BR-27: không được chuyển `status = terminated` khi còn giữ tài sản chưa trả — kiểm tra qua [[Sổ giao–nhận]] trước khi cho đổi trạng thái
