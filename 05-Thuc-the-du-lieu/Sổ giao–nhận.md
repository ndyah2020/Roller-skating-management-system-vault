---
ten: "Sổ giao–nhận"
loai: thực thể
bang_db: custody_log
module_chu: TS
trang_thai: Nháp
tags:
  - thuc-the
---

# Sổ giao–nhận

> Bảng: `custody_log` · Module chủ: TS

Sổ **gộp chung** cho cả tài sản CLB ([[Tài sản]], module TS) và hàng tồn kho ([[Hàng tồn kho]], module BH) — vì cả hai đều "giao cho một HLV ngẫu nhiên giữ", tra một chỗ cho nhanh (QĐ-03). Đây là bảng thay cho `asset_assignment` + `stock_item.holder_staff_id` của bản cũ.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `item_type` | text | — | ✓ | enum: `asset` / `stock_item` — quyết định `item_id` trỏ vào bảng nào |
| `item_id` | uuid | — *(đa hình, không FK cứng)* | ✓ | Trỏ tới `asset.id` hoặc `stock_item.id` tuỳ `item_type` |
| `holder_type` | text | — | ✓ | enum: `staff` / `warehouse` / `student` |
| `holder_id` | uuid | — *(đa hình, không FK cứng)* | – | `staff.id` hoặc `student.id` tuỳ `holder_type`; để trống nếu `warehouse` (đang ở kho, không ai giữ) |
| `assigned_at` | timestamptz | — | ✓ | Lúc giao |
| `due_return_at` | timestamptz | — | – | Hạn trả, nếu có |
| `returned_at` | timestamptz | — | – | Lúc trả — để trống nghĩa là vẫn đang giữ |
| `condition_out` | text | — | – | Tình trạng lúc giao |
| `condition_in` | text | — | – | Tình trạng lúc nhận lại |
| `photo_url` | text | — | – | Ảnh xác nhận |
| `confirmed_by` | uuid | FK → `user.id` | – | Người **nhận** xác nhận lại trên máy họ (S8) |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`item_type`, `item_id`)
- Index: (`holder_type`, `holder_id`, `returned_at`) — phục vụ màn hình S9 "Tôi đang giữ gì" (lọc `returned_at IS NULL`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-05 - Tài sản và hàng hoá]] — BR-26, BR-27, BR-29
- [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]]

## Ghi chú

- `item_id` và `holder_id` là cột đa hình — xem bảng tổng hợp ở [[Sơ đồ dữ liệu]].
