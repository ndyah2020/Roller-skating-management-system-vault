---
ten: "Khách quan tâm"
loai: thực thể
bang_db: lead
module_chu: MK
trang_thai: Nháp
tags:
  - thuc-the
---

# Khách quan tâm

> Bảng: `lead` · Module chủ: MK

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `full_name` | text | — | ✓ | Tên người liên hệ (phụ huynh) |
| `phone` | text | — | – | Số điện thoại |
| `zalo` | text | — | – | Zalo |
| `child_age` | integer | — | – | Tuổi con |
| `area` | text | — | – | Khu vực ở |
| `source` | text | — | – | Facebook / TikTok / giới thiệu / vãng lai / tờ rơi — gợi ý lấy từ [[Danh mục dùng chung]] |
| `status` | text | — | ✓ | enum: `new` / `contacted` / `trial_booked` / `converted` / `lost` |
| `assigned_to_staff_id` | uuid | FK → `staff.id` | – | HLV/nhân viên phụ trách chăm sóc |
| `note` | text | — | – | Ghi chú |
