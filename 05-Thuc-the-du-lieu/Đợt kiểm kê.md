---
ten: "Đợt kiểm kê"
loai: thực thể
bang_db: stock_count
module_chu: TS
trang_thai: Nháp
tags:
  - thuc-the
---

# Đợt kiểm kê

> Bảng: `stock_count` · Module chủ: TS

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
