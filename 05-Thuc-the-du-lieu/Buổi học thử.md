---
ten: "Buổi học thử"
loai: thực thể
bang_db: trial_session
module_chu: MK
trang_thai: Nháp
tags:
  - thuc-the
---

# Buổi học thử

> Bảng: `trial_session` · Module chủ: MK

Khi chuyển đổi thành công, [[Khách quan tâm]] trở thành [[Học viên]] chính thức qua [[Ghi danh]] (P1).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `lead_id` | uuid | FK → `lead.id` | ✓ | Khách quan tâm |
| `session_id` | uuid | FK → `session.id` | – | Buổi học thử ghép vào lịch thật, nếu có |
| `venue_id` | uuid | FK → `venue.id` | ✓ | Sân học thử |
| `staff_id` | uuid | FK → `staff.id` | – | HLV phụ trách buổi thử |
| `trial_date` | date | — | ✓ | Ngày học thử |
| `result` | text | — | – | enum: `attended` / `no_show` / `converted` / `rejected` |
| `reject_reason` | text | — | – | Lý do không chuyển đổi, nếu có |
