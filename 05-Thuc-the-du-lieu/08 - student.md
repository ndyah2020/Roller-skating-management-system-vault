---
ten: "Học viên"
loai: thực thể
bang_db: student
module_chu: HV
so_thu_tu: 8
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Học viên"
---

# student

> #08 · Học viên · Module chủ: HV

Một dòng = một trẻ học patin. Học viên **không có tài khoản đăng nhập** — xem [[Tài khoản đăng nhập]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột                  | Kiểu | Khoá             | Bắt buộc | Mô tả                                                                     |
| -------------------- | ---- | ---------------- | -------- | ------------------------------------------------------------------------- |
| `full_name`          | text | —                | ✓        | Họ tên                                                                    |
| `birth_date`         | date | —                | ✓        | Ngày sinh                                                                 |
| `gender`             | text | —                | –        | enum: `male` / `female` / `other`                                         |
| `photo_url`          | text | —                | –        | Ảnh đại diện                                                              |
| `shoe_size`          | text | —                | –        | Size chân, đồng bộ khi đo lại ở BH                                        |
| `gear_size`          | text | —                | –        | Size bộ bảo hộ                                                            |
| `current_lesson_id`  | uuid | FK → `lesson.id` | –        | Bài học hiện tại — trình độ suy ra qua `lesson.level_id`, không lưu riêng |
| `preferred_staff_id` | uuid | FK → `staff.id`  | –        | HLV mong muốn — chỉ là gợi ý mềm, đổi người khác vẫn hợp lệ (BR-02)       |
| `status`             | text | —                | ✓        | enum: `active` / `paused` / `stopped` — mặc định `active`                 |
| `note`               | text | —                | –        | Ghi chú tự do                                                             |

## Ràng buộc & chỉ mục

- Index: `current_lesson_id`, `preferred_staff_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-02 (`preferred_staff_id` chỉ là gợi ý)

## Ghi chú / câu hỏi mở

- [ ] ❓ `preferred_staff_id` — giữ hay bỏ nếu thấy thừa (ghi chú gốc từ bản đồ module trước).
- Đã bỏ so với bản cũ: `current_level_id` (suy ra qua `current_lesson_id`), quyền sử dụng hình ảnh (`photo_consent`, xem [[Học viên]] — không cần bảng riêng hiện tại).
