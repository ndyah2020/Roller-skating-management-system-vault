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

Một dòng = một **người học** patin — thường là một trẻ (con của phụ huynh, quan hệ ở [[Quan hệ học viên – phụ huynh]]), nhưng cũng có thể là chính phụ huynh tự đăng ký học cho mình (xem cột `guardian_self_id` dưới). Trẻ học viên **không có tài khoản đăng nhập riêng** — xem [[Tài khoản đăng nhập]]; nếu là phụ huynh tự học thì họ vẫn đăng nhập bằng tài khoản phụ huynh sẵn có như thường, `guardian_self_id` chỉ để biết dòng `student` này tương ứng với phụ huynh nào.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `full_name` | text | — | ✓ | Họ tên |
| `birth_date` | date | — | ✓ | Ngày sinh |
| `gender` | text | — | – | enum: `male` / `female` / `other` |
| `photo_url` | text | — | – | Ảnh đại diện |
| `shoe_size` | text | — | – | Size chân, đồng bộ khi đo lại ở BH |
| `gear_size` | text | — | – | Size bộ bảo hộ |
| `current_lesson_id` | uuid | FK → `lesson.id` | – | Bài học hiện tại — trình độ suy ra qua `lesson.level_id`, không lưu riêng |
| `preferred_staff_id` | uuid | FK → `staff.id` | – | HLV mong muốn — chỉ là gợi ý mềm, đổi người khác vẫn hợp lệ (BR-02) |
| `guardian_self_id` | uuid | FK → `guardian.id` | – | Có giá trị khi dòng này là hồ sơ học của **chính phụ huynh đó** tự đăng ký (không phải con của họ) |
| `status` | text | — | ✓ | enum: `active` / `paused` / `stopped` — mặc định `active` |
| `note` | text | — | – | Ghi chú tự do |

## Ràng buộc & chỉ mục

- Index: `current_lesson_id`, `preferred_staff_id`, `guardian_self_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-02 (`preferred_staff_id` chỉ là gợi ý)

## Ghi chú / câu hỏi mở

- [ ] ❓ `preferred_staff_id` — giữ hay bỏ nếu thấy thừa (ghi chú gốc từ bản đồ module trước).
- Đã bỏ so với bản cũ: `current_level_id` (suy ra qua `current_lesson_id`), quyền sử dụng hình ảnh (`photo_consent`, xem [[Học viên]] — không cần bảng riêng hiện tại).
- Thêm `guardian_self_id` — cho phép phụ huynh tự đăng ký học, không chỉ đăng ký cho con. Khi cột này có giá trị, thường sẽ **không** có dòng nào ở [[Quan hệ học viên – phụ huynh]] cho `student` này (không cần ghi "phụ huynh của chính mình") — quy tắc này ở tầng ứng dụng, CSDL không ép. Nhờ tách riêng thế này, toàn bộ `enrollment`, `session`, `student_package`... không cần đổi gì để hỗ trợ phụ huynh tự học. Xem [[QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học]].
