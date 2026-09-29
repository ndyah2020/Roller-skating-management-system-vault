---
ten: "Thuê giày theo buổi"
loai: thực thể
bang_db: equipment_rental
module_chu: TS
so_thu_tu: 45
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Thuê giày theo buổi"
---

# equipment_rental

> #45 · Thuê giày theo buổi · Module chủ: TS

Học viên thuê giày cho một buổi học — sinh khoản thu bên TC qua [[Dòng hoá đơn]] (`item_type = rental`).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `asset_id` | uuid | FK → `asset.id` | ✓ | Giày cho thuê |
| `student_id` | uuid | FK → `student.id` | ✓ | Học viên thuê |
| `session_id` | uuid | FK → `session.id` | – | Buổi thuê, nếu gắn buổi cụ thể |
| `rented_at` | timestamptz | — | ✓ | Lúc thuê |
| `returned_at` | timestamptz | — | – | Lúc trả |
| `fee` | bigint | — | ✓ | Phí thuê — mặc định 0 |
| `invoice_line_id` | uuid | FK → `invoice_line.id` | – | Dòng hoá đơn tương ứng |
