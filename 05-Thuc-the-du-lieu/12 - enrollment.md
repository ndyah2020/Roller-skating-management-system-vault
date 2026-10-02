---
ten: "Ghi danh"
loai: thực thể
bang_db: enrollment
module_chu: HV
so_thu_tu: 12
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Ghi danh"
---

# enrollment

> #12 · Ghi danh · Module chủ: HV

Một dòng = một học viên đăng ký học ở một sân, một khung giờ cố định hằng tuần. Là đầu vào để P2 (xếp lịch tuần) gom số học viên theo ô lịch.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột           | Kiểu     | Khoá              | Bắt buộc | Mô tả                               |
| ------------- | -------- | ----------------- | -------- | ----------------------------------- |
| `student_id`  | uuid     | FK → `student.id` | ✓        | Học viên                            |
| `venue_id`    | uuid     | FK → `venue.id`   | ✓        | Sân đăng ký học                     |
| `course_id`   | uuid     | FK → `course.id`  | ✓        | Loại hình đăng ký (1-1 / nhóm)      |
| `weekday`     | smallint | —                 | ✓        | Thứ trong tuần                      |
| `start_time`  | time     | —                 | ✓        | Giờ bắt đầu                         |
| `start_date`  | date     | —                 | ✓        | Ngày bắt đầu hiệu lực               |
| `end_date`    | date     | —                 | –        | Ngày kết thúc, để trống nếu còn học |
| `status`      | text     | —                 | ✓        | enum: `active` / `paused` / `ended` |
| `stop_reason` | text     | —                 | –        | Lý do dừng, nếu có                  |

## Ràng buộc & chỉ mục

- Index: (`venue_id`, `weekday`, `start_time`) — phục vụ P2 gom theo ô lịch
- Index: `student_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-04: một học viên không được đăng ký hai sân khác nhau trong cùng một ngày. **Khó biểu diễn bằng UNIQUE thuần** (phải so sánh `weekday` giữa các dòng `active` của cùng `student_id` ở `venue_id` khác nhau) — cần kiểm tra ở tầng ứng dụng hoặc trigger, ghi chú lại để không quên khi code.
- Ngày kết thúc khóa học sẽ tự động gắn vào ngay sau khi hoàn thành buổi học cuối cùng
