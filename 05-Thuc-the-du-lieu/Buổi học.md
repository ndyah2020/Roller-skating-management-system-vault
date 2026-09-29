---
ten: "Buổi học"
loai: thực thể
bang_db: session
module_chu: DH
trang_thai: Nháp
tags:
  - thuc-the
---

# Buổi học

> Bảng: `session` · Module chủ: DH

**Bảng trục của toàn hệ thống** — điểm danh, chấm công HLV, trừ buổi trong gói đều móc vào đây (xem [[QT-06 - Dữ liệu và kế toán]]).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `venue_id` | uuid | FK → `venue.id` | ✓ | Sân diễn ra buổi học |
| `start_at` | timestamptz | — | ✓ | Giờ bắt đầu |
| `end_at` | timestamptz | — | ✓ | Giờ kết thúc |
| `booked_course_id` | uuid | FK → `course.id` | ✓ | Loại hình đã đăng ký |
| `actual_course_id` | uuid | FK → `course.id` | – | Loại hình **thực tế dạy** — để trống nghĩa là giống `booked_course_id` (BR-17) |
| `status` | text | — | ✓ | enum: `scheduled` / `done` / `cancelled` / `rescheduled` |
| `cancel_reason` | text | — | – | enum: `mưa` / `hlv_ban` / `san_ban` / `hoc_vien_bao_nghi` / `khac` — **bắt buộc khi `status = cancelled`** (BR-16) |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Check: `end_at` > `start_at`
- Check: `cancel_reason` NOT NULL khi `status = 'cancelled'`
- Index: (`venue_id`, `start_at`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-05, BR-06
- [[QT-03 - Trừ buổi]] — BR-16, BR-17, vì sao cần `cancel_reason`

## Ghi chú

- **Không còn** bảng `makeup_session` (buổi học bù) — đã bỏ, xem note [[DH - Dạy học]].
