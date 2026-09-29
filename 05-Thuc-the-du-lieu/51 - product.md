---
ten: "Sản phẩm"
loai: thực thể
bang_db: product
module_chu: BH
so_thu_tu: 51
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Sản phẩm"
---

# product

> #51 · Sản phẩm · Module chủ: BH

Danh mục giày/bảo hộ/đồng phục/bánh xe… CLB có thể bán, không phải hàng đang tồn (tồn kho thật ở [[Hàng tồn kho]]).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `brand` | text | — | – | Hãng |
| `model` | text | — | – | Model |
| `category` | text | — | – | Loại: giày / bảo hộ / đồng phục / bánh xe… |
| `size` | text | — | – | Size (nếu size cố định cho sản phẩm, không đổi theo đơn) |
| `cost_price` | bigint | — | – | Giá nhập, VND |
| `sell_price` | bigint | — | – | Giá bán, VND |
| `default_supplier_id` | uuid | FK → `supplier.id` | – | Nhà cung cấp mặc định |
