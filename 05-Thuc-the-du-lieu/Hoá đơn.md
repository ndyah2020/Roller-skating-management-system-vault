---
ten: "Hoá đơn"
loai: thực thể
bang_db: invoice
module_chu: TC
trang_thai: Nháp
tags:
  - thuc-the
---

# Hoá đơn

> Bảng: `invoice` · Module chủ: TC

Một dòng = một hoá đơn cho học viên/phụ huynh — có thể gồm nhiều dòng chi tiết ở [[Dòng hoá đơn]] và nhiều lần thu ở [[Khoản thu]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_id` | uuid | FK → `student.id` | ✓ | Học viên liên quan |
| `guardian_id` | uuid | FK → `guardian.id` | – | Người đứng tên nhận hoá đơn |
| `issue_date` | date | — | ✓ | Ngày xuất |
| `total_amount` | bigint | — | ✓ | Tổng tiền, VND |
| `discount_amount` | bigint | — | ✓ | Giảm giá — mặc định 0 |
| `paid_amount` | bigint | — | ✓ | Đã thu — tổng từ [[Khoản thu]], mặc định 0 |
| `status` | text | — | ✓ | enum: `draft` / `issued` / `partially_paid` / `paid` / `void` |

## Ràng buộc & chỉ mục

- Index: `student_id`

## Ghi chú

- `paid_amount` nên tính lại (không lưu cứng) từ tổng [[Khoản thu]] để tránh lệch — hoặc cập nhật bằng trigger nếu lưu cứng cho nhanh truy vấn.
