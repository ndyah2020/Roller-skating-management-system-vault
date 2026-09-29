---
ten: "Chi tiết nhận hàng"
loai: thực thể
bang_db: goods_receipt_line
module_chu: BH
so_thu_tu: 55
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Chi tiết nhận hàng"
---

# goods_receipt_line

> #55 · Chi tiết nhận hàng · Module chủ: BH

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `goods_receipt_id` | uuid | FK → `goods_receipt.id` | ✓ | Phiếu nhập |
| `product_id` | uuid | FK → `product.id` | ✓ | Sản phẩm |
| `size` | text | — | – | Size |
| `quantity_ordered` | integer | — | ✓ | Số lượng đã đặt |
| `quantity_received` | integer | — | ✓ | Số lượng thực nhận |
| `discrepancy_note` | text | — | – | Ghi chú nếu lệch số lượng |
