---
ten: "Chấm công"
loai: thực thể
bang_db: timesheet
module_chu: NS
trang_thai: Nháp
tags:
  - thuc-the
---

# Chấm công

> Bảng: `timesheet` · Module chủ: NS

Một dòng = một lần HLV check-in/check-out tại sân cho một buổi. Là đầu vào chính của [[Bảng công theo kỳ]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV chấm công |
| `session_id` | uuid | FK → `session.id` | – | Buổi tương ứng, nếu chấm công gắn buổi dạy |
| `venue_id` | uuid | FK → `venue.id` | ✓ | Sân check-in |
| `check_in_at` | timestamptz | — | – | Lúc check-in — dùng cả cho BR-07 (lọc điểm danh) |
| `check_out_at` | timestamptz | — | – | Lúc check-out |
| `worked_minutes` | integer | — | – | Số phút làm việc, tính từ check-in/out hoặc theo lịch |
| `variance_minutes` | integer | — | – | Lệch so với lịch — dương/âm tuỳ sớm/muộn |
| `status` | text | — | ✓ | enum: `pending` / `approved` / `rejected` |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`staff_id`, `check_in_at`)
- Index: `session_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-07 (vị trí check-in phục vụ lọc danh sách điểm danh)
- [[QT-04 - Lương]] — nguồn dữ liệu chấm công để tính công
- [[QĐ-04 - HLV tự chấm công bằng check-in, check-out]]
