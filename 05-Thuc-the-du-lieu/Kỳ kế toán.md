---
ten: "Kỳ kế toán"
loai: thực thể
bang_db: accounting_period
module_chu: TC
trang_thai: Nháp
tags:
  - thuc-the
---

# Kỳ kế toán

> Bảng: `accounting_period` · Module chủ: TC

Một dòng = một tháng. Khi khoá (`is_locked = true`), không sửa được số liệu tiền/buổi thuộc kỳ đó nữa (BR-32).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `year` | integer | — | ✓ | Năm |
| `month` | integer | — | ✓ | Tháng (1–12) |
| `closed_at` | timestamptz | — | – | Lúc chốt |
| `closed_by` | uuid | FK → `user.id` | – | Ai chốt |
| `is_locked` | boolean | — | ✓ | Đã khoá hay chưa — mặc định `false` |

## Ràng buộc & chỉ mục

- Unique: (`year`, `month`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-06 - Dữ liệu và kế toán]] — BR-32
