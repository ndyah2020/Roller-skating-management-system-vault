---
ten: "Đợt đặt hàng"
loai: thực thể
bang_db: purchase_batch
module_chu: BH
so_thu_tu: 52
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Đợt đặt hàng"
---

# purchase_batch

> #52 · Đợt đặt hàng · Module chủ: BH

Gom nhiều [[Đơn đặt của khách]] thành một đợt gửi nhà cung cấp khi đủ số lượng/giá trị.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `supplier_id` | uuid | FK → `supplier.id` | ✓ | Nhà cung cấp |
| `ordered_at` | date | — | ✓ | Ngày gửi đặt |
| `expected_date` | date | — | – | Ngày dự kiến hàng về |
| `total_amount` | bigint | — | ✓ | Tổng giá trị đợt — mặc định 0 |
| `status` | text | — | ✓ | enum: `draft` / `ordered` / `received` / `closed` |
