---
ten: "Nhà cung cấp"
loai: thực thể
bang_db: supplier
module_chu: BH
trang_thai: Nháp
tags:
  - thuc-the
---

# Nhà cung cấp

> Bảng: `supplier` · Module chủ: BH

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên nhà cung cấp |
| `contact_name` | text | — | – | Người liên hệ |
| `phone` | text | — | – | Số điện thoại |
| `lead_time_days` | integer | — | – | Số ngày giao hàng thường mất |
| `min_order_quantity` | integer | — | – | Số lượng đặt tối thiểu |
| `return_policy` | text | — | – | Chính sách trả hàng |
