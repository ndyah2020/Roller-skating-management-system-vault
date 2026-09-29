---
ten: "Dòng hoá đơn"
loai: thực thể
bang_db: invoice_line
module_chu: TC
so_thu_tu: 24
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Dòng hoá đơn"
---

# invoice_line

> #24 · Dòng hoá đơn · Module chủ: TC

Một dòng = một khoản mục trong hoá đơn (gói học, sản phẩm, thuê giày…).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `invoice_id` | uuid | FK → `invoice.id` | ✓ | Hoá đơn chứa dòng này |
| `item_type` | text | — | ✓ | enum: `package` / `product` / `rental` / `other` — quyết định `item_id` trỏ vào bảng nào |
| `item_id` | uuid | — *(đa hình, không FK cứng)* | – | Trỏ tới `package.id` / `product.id` / `equipment_rental.id` tuỳ `item_type` |
| `description` | text | — | ✓ | Mô tả dòng |
| `quantity` | numeric(6,2) | — | ✓ | Số lượng — mặc định 1 |
| `unit_price` | bigint | — | ✓ | Đơn giá, VND |
| `amount` | bigint | — | ✓ | Thành tiền = `quantity` × `unit_price` |

## Ràng buộc & chỉ mục

- Index: `invoice_id`

## Ghi chú

- `item_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
