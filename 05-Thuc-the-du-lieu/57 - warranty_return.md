---
ten: "Bảo hành và đổi trả"
loai: thực thể
bang_db: warranty_return
module_chu: BH
so_thu_tu: 57
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Bảo hành và đổi trả"
---

# warranty_return

> #57 · Bảo hành và đổi trả · Module chủ: BH

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `customer_order_id` | uuid | FK → `customer_order.id` | ✓ | Đơn gốc |
| `reason` | text | — | ✓ | Lý do bảo hành/đổi trả |
| `action` | text | — | – | enum: `repair` / `replace` / `refund` |
| `cost` | bigint | — | ✓ | Chi phí xử lý — mặc định 0 |
| `handled_at` | date | — | – | Ngày xử lý |
| `handled_by` | uuid | FK → `user.id` | – | Ai xử lý |
