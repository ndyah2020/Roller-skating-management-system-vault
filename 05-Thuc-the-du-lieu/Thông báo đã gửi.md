---
ten: "Thông báo đã gửi"
loai: thực thể
bang_db: notification_log
module_chu: BC
trang_thai: Nháp
tags:
  - thuc-the
---

# Thông báo đã gửi

> Bảng: `notification_log` · Module chủ: BC

**Log bất biến** — chỉ có `id`, `created_at`; không có `updated_at` / `updated_by` / `is_active`.

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `template_id` | uuid | FK → `notification_template.id` | ✓ | Mẫu đã dùng |
| `recipient_type` | text | — | ✓ | enum: `guardian` / `staff` / `user` — quyết định `recipient_id` trỏ vào bảng nào |
| `recipient_id` | uuid | — *(đa hình, không FK cứng)* | ✓ | Người nhận |
| `channel` | text | — | ✓ | enum: `zalo` / `sms` / `email` |
| `sent_at` | timestamptz | — | ✓ | Lúc gửi |
| `status` | text | — | ✓ | enum: `sent` / `failed` / `read` |
| `read_at` | timestamptz | — | – | Lúc người nhận đọc, nếu theo dõi được |

## Ghi chú

- `recipient_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
