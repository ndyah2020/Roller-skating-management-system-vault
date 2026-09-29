---
ten: "Cảnh báo cần xử lý"
loai: thực thể
bang_db: alert
module_chu: BC
trang_thai: Nháp
tags:
  - thuc-the
---

# Cảnh báo cần xử lý

> Bảng: `alert` · Module chủ: BC

Nguồn cho màn hình S11 (Dashboard) — "việc cần xử lý hiện ngay đầu trang".

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `alert_type` | text | — | ✓ | Ví dụ: `package_low`, `asset_not_returned`, `attendance_not_closed` |
| `reference_type` | text | — | ✓ | Bảng liên quan, ví dụ `student_package`, `custody_log`, `session` |
| `reference_id` | uuid | — *(đa hình, không FK cứng)* | ✓ | Bản ghi cụ thể gây ra cảnh báo |
| `severity` | text | — | ✓ | enum: `info` / `warning` / `critical` — mặc định `info` |
| `assigned_to_user_id` | uuid | FK → `user.id` | – | Ai cần xử lý |
| `is_resolved` | boolean | — | ✓ | Đã xử lý chưa — mặc định `false` |
| `resolved_at` | timestamptz | — | – | Lúc xử lý xong |

## Ghi chú

- `reference_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
- `attendance_not_closed` gắn với BR-15 — báo khi Quản lý chưa xác nhận số học viên đã học trong ngày. Xem [[QT-03 - Trừ buổi]].
