---
ten: "Bài học"
loai: thực thể
bang_db: lesson
module_chu: DH
trang_thai: Nháp
tags:
  - thuc-the
---

# Bài học

> Bảng: `lesson` · Module chủ: DH

Một dòng = một mốc kỹ năng cố định theo thứ tự (chạy thắng, đi cốc, đi lùi…). **Gộp từ `skill` cũ** — không còn bảng `skill` riêng.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột          | Kiểu    | Khoá            | Bắt buộc | Mô tả                         |
| ------------ | ------- | --------------- | -------- | ----------------------------- |
| `name`       | text    | —               | ✓        | Tên bài, ví dụ "Chạy thắng"   |
| `level_id`   | uuid    | FK → `level.id` | ✓        | Trình độ chứa bài này         |
| `sort_order` | integer | —               | ✓        | Thứ tự bài trong chương trình |
| `criteria`   | text    | —               | –        | Tiêu chí đạt bài              |


## Ràng buộc & chỉ mục

- Index: `level_id`
- Unique gợi ý: (`level_id`, `sort_order`)

## Ghi chú / câu hỏi mở

- [ ] ❓ Xác nhận gộp `skill` vào `lesson` là quyết định cuối  nếu sau này cần phân biệt "bài học" (buổi dạy gì) và "kỹ năng" (tiêu chí đánh giá) độc lập, tách lại thành 2 bảng.
