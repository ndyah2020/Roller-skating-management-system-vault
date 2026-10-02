---
ten: "HLV được phân cho buổi"
loai: thực thể
bang_db: session_coach
module_chu: DH
so_thu_tu: 17
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "HLV được phân cho buổi"
---

# session_coach

> #17 · HLV được phân cho buổi · Module chủ: DH

Một buổi có thể có nhiều HLV. **Không** ghi HLV nào kèm bé nào — chỉ ghi buổi đó có đủ HLV hay không (BR-02). Quan hệ "ai dạy bé nào" suy ra sau, từ [[Điểm danh]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột          | Kiểu | Khoá              | Bắt buộc | Mô tả                                                                   |
| ------------ | ---- | ----------------- | -------- | ----------------------------------------------------------------------- |
| `session_id` | uuid | FK → `session.id` | ✓        | Buổi học                                                                |
| `staff_id`   | uuid | FK → `staff.id`   | ✓        | HLV được phân                                                           |
| `role`       | text | —                 | –        | enum: `primary` / `assistant` — để trống lúc tạo lịch cũng được (BR-02) |

## Ràng buộc & chỉ mục

- Unique: (`session_id`, `staff_id`)
- Index: `staff_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — BR-02, BR-03, BR-06 (một HLV không trùng giờ hai buổi — **khó biểu diễn bằng UNIQUE thuần**, phải so `session.start_at/end_at` của các buổi cùng `staff_id`, cần kiểm tra ở tầng ứng dụng hoặc trigger)

## Ghi chú

- Đã bác việc thêm bảng ghép nhóm học viên–HLV — xem thay đổi #7 trong bảng phân tích hệ thống (tài liệu project, không phải note trong vault này) — không tạo bảng `session_group`.
