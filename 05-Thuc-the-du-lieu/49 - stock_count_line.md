---
ten: "Chi tiết kiểm kê"
loai: thực thể
bang_db: stock_count_line
module_chu: TS
so_thu_tu: 49
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Chi tiết kiểm kê"
---

# stock_count_line

> #49 · Chi tiết kiểm kê · Module chủ: TS

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `stock_count_id` | uuid | FK → `stock_count.id` | ✓ | Đợt kiểm kê |
| `asset_id` | uuid | FK → `asset.id` | ✓ | Tài sản được kiểm |
| `book_quantity` | integer | — | ✓ | Số lượng trên sổ |
| `actual_quantity` | integer | — | ✓ | Số lượng thực tế đếm được |
| `variance` | integer | — | ✓ | = `actual_quantity` − `book_quantity` |
| `handling_note` | text | — | – | Cách xử lý nếu lệch |

## Ràng buộc & chỉ mục

- Unique: (`stock_count_id`, `asset_id`)
