---
ten: "Quan hệ học viên – phụ huynh"
loai: thực thể
bang_db: student_guardian
module_chu: HV
so_thu_tu: 10
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Quan hệ học viên – phụ huynh"
---

# student_guardian

> #10 · Quan hệ học viên – phụ huynh · Module chủ: HV

Bảng nối nhiều-nhiều: một phụ huynh có thể có nhiều con, một học viên có thể có nhiều người giám hộ.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột            | Kiểu    | Khoá               | Bắt buộc | Mô tả                                               |
| -------------- | ------- | ------------------ | -------- | --------------------------------------------------- |
| `student_id`   | uuid    | FK → `student.id`  | ✓        | Học viên                                            |
| `guardian_id`  | uuid    | FK → `guardian.id` | ✓        | Phụ huynh/người giám hộ                             |
| `relationship` | text    | —                  | ✓        | enum: `mother` / `father` / `grandparent` / `other` |
| `is_payer`     | boolean | —                  | ✓        | Có phải người đóng tiền chính mặc định `false`      |

## Ràng buộc & chỉ mục

- Unique: (`student_id`, `guardian_id`)

## Ghi chú

- Đã bỏ so với bản cũ: `can_pick_up` (người được phép đón trẻ) — hiện chưa cần.
