---
ten: "Tiến độ bài học của học viên"
loai: thực thể
bang_db: student_lesson
module_chu: DH
so_thu_tu: 19
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Tiến độ bài học của học viên"
---

# student_lesson

> #19 · Tiến độ bài học của học viên · Module chủ: DH

Một dòng = tình trạng học một bài của một học viên. **Đổi tên từ `student_skill` cũ** theo cùng quyết định gộp `skill` vào `lesson`.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_id` | uuid | FK → `student.id` | ✓ | Học viên |
| `lesson_id` | uuid | FK → `lesson.id` | ✓ | Bài học |
| `status` | text | — | ✓ | enum: `not_started` / `learning` / `achieved` — mặc định `not_started` |
| `assessed_at` | timestamptz | — | – | Lúc đánh giá |
| `assessed_by_staff_id` | uuid | FK → `staff.id` | – | HLV đánh giá |
| `note` | text | — | – | Nhận xét |
| `media_url` | text | — | – | Ảnh/video minh chứng |

## Ràng buộc & chỉ mục

- Unique: (`student_id`, `lesson_id`)

## Ghi chú

- `student.current_lesson_id` chỉ trỏ tới bài **đang học hiện tại**; lịch sử toàn bộ bài đã/đang học nằm ở bảng này.
