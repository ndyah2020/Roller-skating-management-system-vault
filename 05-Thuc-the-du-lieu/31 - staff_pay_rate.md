---
ten: "Mức lương theo giờ"
loai: thực thể
bang_db: staff_pay_rate
module_chu: NS
so_thu_tu: 31
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Mức lương theo giờ"
---

# staff_pay_rate

> #31 · Mức lương theo giờ · Module chủ: NS

Giữ lịch sử khi đổi lương — không sửa dòng cũ, thêm dòng mới với `effective_from` mới.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV |
| `hourly_rate` | bigint | — | ✓ | Đơn giá mỗi giờ, VND |
| `effective_from` | date | — | ✓ | Áp dụng từ ngày |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`staff_id`, `effective_from`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]]

## Ghi chú

- Đơn giá này chỉ dùng khi cụm ca được chọn là **ca thường**. Cụm chọn **ca lẻ** dùng mức cố định ở [[Quy tắc tính công]] thay vào, không dùng đơn giá này. Xem [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]].
