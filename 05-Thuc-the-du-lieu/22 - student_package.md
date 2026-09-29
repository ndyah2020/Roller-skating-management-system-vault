---
ten: "Gói học của học viên"
loai: thực thể
bang_db: student_package
module_chu: TC
so_thu_tu: 22
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Gói học của học viên"
---

# student_package

> #22 · Gói học của học viên · Module chủ: TC

Một dòng = một lần một học viên mua một gói, theo dõi số buổi còn lại. Bị trừ/hoàn qua [[Biến động số buổi]], không sửa trực tiếp `session_remaining` ở nơi khác.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_id` | uuid | FK → `student.id` | ✓ | Học viên |
| `package_id` | uuid | FK → `package.id` | ✓ | Gói đã mua |
| `purchased_at` | timestamptz | — | ✓ | Lúc mua |
| `session_total` | numeric(5,2) | — | ✓ | Tổng số buổi của gói lúc mua |
| `session_remaining` | numeric(5,2) | — | ✓ | Số buổi còn lại — chỉ đổi qua [[Biến động số buổi]] đã `is_confirmed` |
| `expire_at` | date | — | – | Ngày hết hạn |
| `status` | text | — | ✓ | enum: `active` / `expired` / `exhausted` / `cancelled` |

## Ràng buộc & chỉ mục

- Check: `session_remaining` ≥ 0
- Index: `student_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-03 - Trừ buổi]]
