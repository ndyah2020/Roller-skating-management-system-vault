---
ten: "Sân dạy học"
loai: thực thể
bang_db: venue
module_chu: HT
so_thu_tu: 5
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Sân dạy học"
---

# venue

> #05 · Sân dạy học · Module chủ: HT

Một dòng = một địa điểm CLB có dạy. **Không có cột sức chứa cố định** — sức chứa một khung giờ suy ra động từ số HLV được phân vào khung giờ đó, xem BR-03 ở [[QT-01 - Lớp và lịch]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên sân |
| `address` | text | — | ✓ | Địa chỉ |
| `status` | text | — | ✓ | enum: `active` / `inactive` — mặc định `active` |
| `note` | text | — | – | Ghi chú tự do |

## Ràng buộc & chỉ mục

- Unique: `name` (khuyến nghị, tránh trùng tên sân gây nhầm khi chọn trên điện thoại)

## Ghi chú

- Khung giờ dùng được của sân nằm ở bảng riêng [[Khung giờ của sân]], không nhồi vào đây.
