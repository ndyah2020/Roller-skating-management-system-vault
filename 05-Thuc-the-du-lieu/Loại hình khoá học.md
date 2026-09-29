---
ten: "Loại hình khoá học"
loai: thực thể
bang_db: course
module_chu: DH
trang_thai: Nháp
tags:
  - thuc-the
---

# Loại hình khoá học

> Bảng: `course` · Module chủ: DH

Một dòng = một cách dạy: 1-1, nhóm, hoặc nửa 1-1 nửa nhóm. `session.booked_course_id` / `actual_course_id` và `package.course_id` đều trỏ vào đây.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên hiển thị |
| `teaching_mode` | text | — | ✓ | enum: `one_on_one` / `group` / `half_one_on_one_half_group` |
| `max_student_per_coach` | integer | — | ✓ | 1 nếu 1-1, 4 nếu nhóm (BR-01) |
| `duration_minutes` | integer | — | ✓ | Mặc định 60 |
| `description` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Check: `max_student_per_coach` ≥ 1

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-01
- [[QĐ-07 - Nửa buổi 1-1 chuyển nhóm, CLB bù phần thiếu HLV]] — lý do có giá trị `half_one_on_one_half_group`

## Ghi chú / câu hỏi mở

- [ ] ❓ `half_one_on_one_half_group` để là một `course` riêng (đã chọn ở đây) — đối chiếu lại với [[QĐ-07 - Nửa buổi 1-1 chuyển nhóm, CLB bù phần thiếu HLV]] khi chốt.
