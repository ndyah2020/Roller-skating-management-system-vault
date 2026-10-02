---
ten: "Trình độ"
loai: thực thể
bang_db: level
module_chu: DH
so_thu_tu: 14
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Trình độ"
---

# level

> #14 · Trình độ · Module chủ: DH

Nhóm các bài học ([[Bài học]]) theo thứ tự trình độ, ví dụ "Cơ bản", "Nâng cao".

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột           | Kiểu    | Khoá | Bắt buộc | Mô tả                  |
| ------------- | ------- | ---- | -------- | ---------------------- |
| `name`        | text    | —    | ✓        | Tên trình độ           |
| `sort_order`  | integer | —    | ✓        | Thứ tự từ thấp tới cao |
| `description` | text    | —    | –        | Ghi chú                |

## Ghi chú

- Trình độ hiện tại của học viên **không lưu trực tiếp** — suy ra qua `student.current_lesson_id → lesson.level_id`.
