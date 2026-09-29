---
ten: "Đánh giá HLV"
loai: thực thể
bang_db: staff_review
module_chu: NS
trang_thai: Nháp
tags:
  - thuc-the
---

# Đánh giá HLV

> Bảng: `staff_review` · Module chủ: NS · *(tuỳ chọn)*

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV được đánh giá |
| `reviewed_at` | date | — | ✓ | Ngày đánh giá |
| `reviewer_by` | uuid | FK → `user.id` | – | Ai đánh giá — thường là Admin, nên trỏ `user.id` chứ không phải `staff.id` |
| `rating` | smallint | — | – | Điểm đánh giá |
| `comment` | text | — | – | Nhận xét |

## Ghi chú

- Đã đổi `reviewer_staff_id` (bản cũ) → `reviewer_by` trỏ `user.id`, vì người đánh giá thường là Admin (không có hồ sơ `staff`) — xem [[Sơ đồ dữ liệu]].
