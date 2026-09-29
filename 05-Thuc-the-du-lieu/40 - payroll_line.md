---
ten: "Dòng cấu thành lương"
loai: thực thể
bang_db: payroll_line
module_chu: LG
so_thu_tu: 40
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Dòng cấu thành lương"
---

# payroll_line

> #40 · Dòng cấu thành lương · Module chủ: LG

Một dòng = một khoản cụ thể cộng/trừ vào lương (giờ công, ca lẻ, thưởng, khấu trừ…) — để Admin xem và sửa từng dòng trước khi chốt (màn hình S7).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `payroll_id` | uuid | FK → `payroll.id` | ✓ | Bảng công chứa dòng này |
| `line_type` | text | — | ✓ | enum: `hourly` / `session` / `odd_shift` / `bonus` / `deduction` |
| `description` | text | — | – | Diễn giải |
| `quantity` | numeric(6,2) | — | – | Số lượng (giờ/buổi/ca) |
| `unit_amount` | bigint | — | – | Đơn giá |
| `amount` | bigint | — | ✓ | Thành tiền — dương là cộng, âm là trừ |
| `reference_type` | text | — | – | Ví dụ `timesheet`, `asset_maintenance` — quyết định `reference_id` trỏ vào bảng nào |
| `reference_id` | uuid | — *(đa hình, không FK cứng)* | – | Bản ghi gốc sinh ra dòng này |

## Ràng buộc & chỉ mục

- Index: `payroll_id`

## Ghi chú

- `reference_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
- `line_type` là nơi lựa chọn ca thường (`hourly`) hay ca lẻ (`odd_shift`) trở thành dòng lương thật — chọn thủ công theo từng cụm ca, mặc định `hourly`. Xem [[QT-04 - Lương]] (BR-24, BR-25).
