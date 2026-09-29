---
ten: "Ghi chú y tế"
loai: thực thể
bang_db: medical_note
module_chu: HV
so_thu_tu: 11
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Ghi chú y tế"
---

# medical_note

> #11 · Ghi chú y tế · Module chủ: HV · *(tuỳ chọn)*

Quan hệ 1–1 với học viên, tách bảng riêng để dễ giới hạn quyền xem (dữ liệu nhạy cảm) và dễ bỏ hẳn nếu CLB chưa cần.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_id` | uuid | FK → `student.id`, unique | ✓ | Một học viên chỉ có một dòng |
| `allergy` | text | — | – | Dị ứng |
| `chronic_condition` | text | — | – | Bệnh mãn tính |
| `medication` | text | — | – | Thuốc đang dùng |
| `activity_note` | text | — | – | Hạn chế vận động cần lưu ý |
| `blood_type` | text | — | – | Nhóm máu |

## Ràng buộc & chỉ mục

- Unique: `student_id` (đảm bảo 1–1)

## Ghi chú

- Bảng *tuỳ chọn* — có thể để trống hoàn toàn nếu CLB chưa cần thu thập.
