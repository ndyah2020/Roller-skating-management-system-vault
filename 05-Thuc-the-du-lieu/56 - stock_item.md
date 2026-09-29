---
ten: "Hàng tồn kho"
loai: thực thể
bang_db: stock_item
module_chu: BH
so_thu_tu: 56
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Hàng tồn kho"
---

# stock_item

> #56 · Hàng tồn kho · Module chủ: BH

Hàng đặt dư (đặt nhiều hơn số khách chốt mua), giữ lại bán tiếp. Ai đang giữ **không** lưu ở đây nữa — tra ở [[Sổ giao–nhận]] (`item_type = stock_item`), theo QĐ-03.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `product_id` | uuid | FK → `product.id` | ✓ | Sản phẩm |
| `size` | text | — | – | Size |
| `source_batch_id` | uuid | FK → `purchase_batch.id` | – | Đợt đặt hàng sinh ra hàng dư này |
| `held_since` | date | — | – | Có từ ngày nào |
| `cost_price` | bigint | — | – | Giá nhập, VND |
| `status` | text | — | ✓ | enum: `in_stock` / `reserved` / `sold` / `returned_to_supplier` |
| `sold_order_id` | uuid | FK → `customer_order.id` | – | Đơn đã bán được, nếu `status = sold` |

## Ghi chú

- Đã bỏ cột `holder_staff_id` (bản cũ) — người giữ nay tra qua [[Sổ giao–nhận]].

## Quy tắc nghiệp vụ áp dụng

- [[QT-05 - Tài sản và hàng hoá]] — BR-28
