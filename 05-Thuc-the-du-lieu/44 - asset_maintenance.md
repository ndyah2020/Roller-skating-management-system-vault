---
ten: "Bảo trì tài sản"
loai: thực thể
bang_db: asset_maintenance
module_chu: TS
so_thu_tu: 44
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Bảo trì tài sản"
---

# asset_maintenance

> #44 · Bảo trì tài sản · Module chủ: TS

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột                | Kiểu   | Khoá            | Bắt buộc | Mô tả                               |
| ------------------ | ------ | --------------- | -------- | ----------------------------------- |
| `asset_id`         | uuid   | FK → `asset.id` | ✓        | Tài sản được bảo trì                |
| `maintenance_date` | date   | —               | ✓        | Ngày bảo trì                        |
| `description`      | text   | —               | ✓        | Nội dung (thay bánh, thay bạc đạn…) |
| `cost`             | bigint | —               | ✓        | Chi phí — mặc định 0                |
| `vendor`           | text   | —               | –        | Nơi sửa                             |
| `result`           | text   | —               | –        | Kết quả                             |
