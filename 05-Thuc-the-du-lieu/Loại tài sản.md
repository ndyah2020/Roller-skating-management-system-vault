---
ten: "Loại tài sản"
loai: thực thể
bang_db: asset_type
module_chu: TS
trang_thai: Nháp
tags:
  - thuc-the
---

# Loại tài sản

> Bảng: `asset_type` · Module chủ: TS

Danh mục loại tài sản: giày patin thử, mũ, bộ bảo hộ, cọc slalom, loa, balo…

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên loại |
| `track_individually` | boolean | — | ✓ | Có theo dõi từng chiếc riêng bằng mã không — mặc định `false` |
| `depreciation_months` | integer | — | – | Số tháng khấu hao, nếu cần tính |
