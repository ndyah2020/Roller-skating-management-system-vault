---
ten: "Hoá đơn"
loai: thực thể
bang_db: invoice
module_chu: TC
so_thu_tu: 23
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Hoá đơn"
---

# invoice

> #23 · Hoá đơn · Module chủ: TC

Một dòng = một hoá đơn cho **một phụ huynh** — gộp chung mọi khoá học/sản phẩm phụ huynh đó đăng ký/mua trong cùng một lần, dù cho bao nhiêu người học (con của họ, hoặc chính họ tự học — xem [[Học viên]]). Chi tiết từng khoản mục và thuộc về người học nào nằm ở [[Dòng hoá đơn]]; có thể thu nhiều lần ở [[Khoản thu]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `guardian_id` | uuid | FK → `guardian.id` | ✓ | Phụ huynh đứng ra thanh toán — chủ thể của hoá đơn |
| `issue_date` | date | — | ✓ | Ngày xuất |
| `total_amount` | bigint | — | ✓ | Tổng tiền, VND — tổng các dòng ở [[Dòng hoá đơn]] |
| `discount_amount` | bigint | — | ✓ | Giảm giá — mặc định 0 |
| `paid_amount` | bigint | — | ✓ | Đã thu — tổng từ [[Khoản thu]], mặc định 0 |
| `status` | text | — | ✓ | enum: `draft` / `issued` / `partially_paid` / `paid` / `void` |

## Ràng buộc & chỉ mục

- Index: `guardian_id`

## Ghi chú

- `paid_amount` nên tính lại (không lưu cứng) từ tổng [[Khoản thu]] để tránh lệch — hoặc cập nhật bằng trigger nếu lưu cứng cho nhanh truy vấn.
- Đã bỏ `student_id` khỏi bảng này (bản cũ gộp thẳng vào đây, 1 hoá đơn chỉ gắn 1 học viên) — chuyển xuống [[Dòng hoá đơn]] vì 1 hoá đơn giờ gộp được nhiều người học cùng lúc (ví dụ phụ huynh đăng ký cho 2 con trong 1 lần, nhận 1 hoá đơn). `guardian_id` đổi thành bắt buộc vì giờ là chủ thể chính của hoá đơn, không còn là thông tin phụ. Xem [[QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học]].
