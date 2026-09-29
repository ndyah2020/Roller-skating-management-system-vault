---
ten: "Tài sản"
loai: thực thể
bang_db: asset
module_chu: TS
trang_thai: Nháp
tags:
  - thuc-the
---

# Tài sản

> Bảng: `asset` · Module chủ: TS

Một dòng = một tài sản của CLB (hoặc một lô, nếu `asset_type.track_individually = false`). Ai đang giữ tra ở [[Sổ giao–nhận]], không lưu ở đây.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `asset_code` | text | — | – | Mã riêng, có khi `track_individually = true`; duy nhất nếu có |
| `asset_type_id` | uuid | FK → `asset_type.id` | ✓ | Loại tài sản |
| `brand` | text | — | – | Hãng |
| `model` | text | — | – | Model |
| `size` | text | — | – | Size, nếu là giày |
| `purchase_date` | date | — | – | Ngày mua |
| `purchase_price` | bigint | — | – | Giá mua, VND |
| `default_venue_id` | uuid | FK → `venue.id` | – | Sân "nhà" của tài sản, nếu có |
| `condition` | text | — | – | Tình trạng hiện tại (mô tả tự do) |
| `status` | text | — | ✓ | enum: `available` / `assigned` / `maintenance` / `lost` / `disposed` |

## Ràng buộc & chỉ mục

- Unique: `asset_code` (khi khác NULL)

## Quy tắc nghiệp vụ áp dụng

- [[QT-05 - Tài sản và hàng hoá]]
- [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]]
