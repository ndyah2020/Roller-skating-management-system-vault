---
ten: "Đơn đặt của khách"
loai: thực thể
bang_db: customer_order
module_chu: BH
trang_thai: Nháp
tags:
  - thuc-the
---

# Đơn đặt của khách

> Bảng: `customer_order` · Module chủ: BH

Một dòng = một đơn khách đặt giày. Luồng trạng thái đi từ tư vấn tới giao hàng, xem [[BH - Bán hàng và đặt giày]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_id` | uuid | FK → `student.id` | – | Học viên đặt, nếu là học viên CLB |
| `guardian_id` | uuid | FK → `guardian.id` | – | Phụ huynh đặt |
| `product_id` | uuid | FK → `product.id` | ✓ | Sản phẩm đặt |
| `size` | text | — | – | Size khách chọn |
| `quantity` | integer | — | ✓ | Số lượng — mặc định 1 |
| `agreed_price` | bigint | — | ✓ | Giá đã thống nhất, VND |
| `deposit_amount` | bigint | — | ✓ | Tiền cọc — mặc định 0 |
| `status` | text | — | ✓ | enum: `consulting` / `awaiting_deposit` / `deposited` / `batched` / `ordered` / `in_transit` / `received` / `delivered` / `cancelled` / `not_taken` |
| `promised_date` | date | — | – | Ngày hẹn giao |
| `purchase_batch_id` | uuid | FK → `purchase_batch.id` | – | Đợt đặt hàng chứa đơn này |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: `purchase_batch_id`, `status`

## Quy tắc nghiệp vụ áp dụng

- [[QT-05 - Tài sản và hàng hoá]] — BR-29
