---
ten: "Đợt kiểm kê"
loai: thực thể
bang_db: stock_count
module_chu: TS
so_thu_tu: 48
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Đợt kiểm kê"
---

# stock_count

> #48 · Đợt kiểm kê · Module chủ: TS

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `counted_at` | date | — | ✓ | Ngày kiểm kê |
| `counted_by` | uuid | FK → `user.id` | ✓ | Ai kiểm kê |
| `status` | text | — | ✓ | enum: `in_progress` / `done` |
| `note` | text | — | – | Ghi chú |

## Ghi chú

- Chi tiết từng tài sản kiểm kê nằm ở [[Chi tiết kiểm kê]].
