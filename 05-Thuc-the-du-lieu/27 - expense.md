---
ten: "Khoản chi"
loai: thực thể
bang_db: expense
module_chu: TC
so_thu_tu: 27
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Khoản chi"
---

# expense

> #27 · Khoản chi · Module chủ: TC

Một dòng = một khoản CLB chi ra: lương, mua sắm, marketing… Có thể tự sinh từ module khác (ví dụ [[Phiếu lương]]) hoặc nhập tay.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `expense_type` | text | — | ✓ | Loại chi phí — enum cố định (lương · mua sắm · marketing…) |
| `venue_id` | uuid | FK → `venue.id` | – | Sân liên quan, nếu có |
| `amount` | bigint | — | ✓ | Số tiền, VND |
| `spent_at` | date | — | ✓ | Ngày chi |
| `spent_by` | uuid | FK → `user.id` | ✓ | Ai chi |
| `approved_by` | uuid | FK → `user.id` | – | Ai duyệt |
| `reference_type` | text | — | – | enum: `payslip` / `asset` / `purchase_batch` / `campaign` — quyết định `reference_id` trỏ vào bảng nào |
| `reference_id` | uuid | — *(đa hình, không FK cứng)* | – | Trỏ tới bản ghi gốc sinh ra khoản chi này |
| `receipt_url` | text | — | – | Ảnh biên nhận |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`reference_type`, `reference_id`)

## Ghi chú

- `reference_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
