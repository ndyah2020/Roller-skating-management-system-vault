---
ten: "Giới thiệu bạn bè"
loai: thực thể
bang_db: referral
module_chu: MK
trang_thai: Nháp
tags:
  - thuc-the
---

# Giới thiệu bạn bè

> Bảng: `referral` · Module chủ: MK

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `referrer_type` | text | — | ✓ | enum: `guardian` / `staff` — quyết định `referrer_id` trỏ vào bảng nào |
| `referrer_id` | uuid | — *(đa hình, không FK cứng)* | ✓ | `guardian.id` hoặc `staff.id` tuỳ `referrer_type` |
| `student_id` | uuid | FK → `student.id` | ✓ | Học viên mới do giới thiệu này mang lại |
| `reward_amount` | bigint | — | ✓ | Tiền thưởng — mặc định 0 |
| `is_paid` | boolean | — | ✓ | Đã trả thưởng chưa — mặc định `false` |
| `paid_at` | date | — | – | Ngày trả thưởng |

## Ghi chú

- `referrer_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
