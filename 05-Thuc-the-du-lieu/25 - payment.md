---
ten: "Khoản thu"
loai: thực thể
bang_db: payment
module_chu: TC
so_thu_tu: 25
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Khoản thu"
---

# payment

> #25 · Khoản thu · Module chủ: TC

Một dòng = một lần thu tiền cho một hoá đơn. Một hoá đơn có thể thu nhiều lần (trả góp, đặt cọc rồi thanh toán đủ…).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `invoice_id` | uuid | FK → `invoice.id` | ✓ | Hoá đơn được thu |
| `amount` | bigint | — | ✓ | Số tiền thu, VND |
| `method` | text | — | ✓ | enum: `cash` / `transfer` / `qr` |
| `paid_at` | timestamptz | — | ✓ | Lúc thu |
| `received_by` | uuid | FK → `user.id` | ✓ | Ai thu (thường là Admin) |
| `reference_no` | text | — | – | Mã tham chiếu chuyển khoản/QR |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: `invoice_id`
