---
ten: "Chi tiết nhận hàng"
loai: thực thể
bang_db: goods_receipt_line
module_chu: BH
trang_thai: Nháp
tags:
  - thuc-the
---

# Chi tiết nhận hàng

> Bảng: `goods_receipt_line` · Module chủ: BH

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
