---
ten: "Gói học"
loai: thực thể
bang_db: package
module_chu: TC
so_thu_tu: 21
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Gói học"
---

# package

> #21 · Gói học · Module chủ: TC

Một dòng = một loại gói CLB bán (theo buổi, theo khoá, combo). Khi học viên mua, sinh một dòng ở [[Gói học của học viên]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên gói |
| `session_count` | integer | — | ✓ | Số buổi — mặc định 10 (BR-13) |
| `valid_days` | integer | — | – | Số ngày hiệu lực kể từ ngày mua, để trống nếu không giới hạn |
| `price` | bigint | — | ✓ | Giá bán, VND |
| `course_id` | uuid | FK → `course.id` | ✓ | Loại hình khoá học đi kèm |
| `description` | text | — | – | Ghi chú |

## Quy tắc nghiệp vụ áp dụng

- [[QT-03 - Trừ buổi]] — BR-13
