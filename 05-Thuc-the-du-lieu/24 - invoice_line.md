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

Một dòng = một khoản mục trong hoá đơn (gói học, sản phẩm, thuê giày…), gắn với đúng một người học nếu khoản mục đó là của riêng ai.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột           | Kiểu         | Khoá                         | Bắt buộc | Mô tả                                                                                                                 |
| ------------- | ------------ | ---------------------------- | -------- | --------------------------------------------------------------------------------------------------------------------- |
| `invoice_id`  | uuid         | FK → `invoice.id`            | ✓        | Hoá đơn chứa dòng này                                                                                                 |
| `student_id`  | uuid         | FK → `student.id`            | –        | Người học khoản mục này dành cho — để trống nếu dòng không gắn với một người học cụ thể (ví dụ bán lẻ sản phẩm chung) |
| `item_type`   | text         | —                            | ✓        | enum: `package` / `product` / `rental` / `other` — quyết định `item_id` trỏ vào bảng nào                              |
| `item_id`     | uuid         | — *(đa hình, không FK cứng)* | –        | Trỏ tới `package.id` / `product.id` / `equipment_rental.id` tuỳ `item_type`                                           |
| `description` | text         | —                            | ✓        | Mô tả dòng                                                                                                            |
| `quantity`    | numeric(6,2) | —                            | ✓        | Số lượng — mặc định 1                                                                                                 |
| `unit_price`  | bigint       | —                            | ✓        | Đơn giá, VND                                                                                                          |
| `amount`      | bigint       | —                            | ✓        | Thành tiền = `quantity` × `unit_price`                                                                                |
|               |              |                              |          |                                                                                                                       |

## Ràng buộc & chỉ mục

- Index: `invoice_id`
- Index: `student_id`

## Ghi chú

- `item_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
- `student_id` mới thêm — vì 1 hoá đơn giờ gộp được nhiều người học (xem [[Hoá đơn]]), mỗi dòng phải tự ghi rõ của ai. "Người học" ở đây gồm cả trường hợp phụ huynh tự đăng ký học cho chính mình (cột `guardian_self_id` ở [[Học viên]]), không chỉ con của họ. Loại hình khoá học (1-1/nhóm…) không cần thêm cột — suy ra qua `item_id` (khi là `package`) → `package.course_id` → `course`. Xem [[QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học]].
