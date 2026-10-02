---
ten: "Điểm danh"
loai: thực thể
bang_db: attendance
module_chu: DH
so_thu_tu: 18
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Điểm danh"
---

# attendance

> #18 · Điểm danh · Module chủ: DH

Một dòng = một học viên trong một buổi, có/không có mặt. `marked_by` là nguồn duy nhất trả lời "ai dạy bé nào" (BR-08).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột                 | Kiểu        | Khoá              | Bắt buộc | Mô tả                                                                         |
| ------------------- | ----------- | ----------------- | -------- | ----------------------------------------------------------------------------- |
| `session_id`        | uuid        | FK → `session.id` | ✓        | Buổi học                                                                      |
| `student_id`        | uuid        | FK → `student.id` | ✓        | Học viên                                                                      |
| `status`            | text        | —                 | ✓        | enum: `present` / `absent` / `partial` / `not_marked` — mặc định `not_marked` |
| `marked_by`         | uuid        | FK → `user.id`    | –        | HLV bấm tích — null nếu `not_marked`. Từ đây biết ai dạy bé nào (BR-08)       |
| `marked_at`         | timestamptz | —                 | –        | Lúc bấm                                                                       |
| `client_request_id` | text        | —                 | –        | Mã lần gửi do máy sinh — lớp chống bấm trùng khi mạng chập chờn (BR-11)       |
| `note`              | text        | —                 | –        | Ghi chú, ví dụ học viên vãng lai thêm giữa buổi                               |

## Ràng buộc & chỉ mục

- **Unique: (`session_id`, `student_id`) — bắt buộc, đây là lớp chống tích trùng chắc nhất (BR-10)**
- Index: `marked_by`

## Quy tắc nghiệp vụ áp dụng

- [[QT-02 - Điểm danh]] — toàn bộ BR-07 đến BR-12, đặc biệt bốn lớp chống tích trùng

## Ghi chú

- Đã bỏ cột `check_in_at` (bản cũ) — trùng ý nghĩa với `marked_at`; check-in thật của HLV nằm ở [[Chấm công]].`timesheet.check_in_at`.
