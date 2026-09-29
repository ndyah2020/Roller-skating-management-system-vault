---
ten: "Phiếu nhập hàng"
loai: thực thể
bang_db: goods_receipt
module_chu: BH
so_thu_tu: 54
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Phiếu nhập hàng"
---

# goods_receipt

> #54 · Phiếu nhập hàng · Module chủ: BH

Ghi nhận hàng về theo một [[Đợt đặt hàng]]. Chi tiết từng sản phẩm ở [[Chi tiết nhận hàng]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `purchase_batch_id` | uuid | FK → `purchase_batch.id` | ✓ | Đợt đặt hàng |
| `received_at` | date | — | ✓ | Ngày nhận |
| `received_by` | uuid | FK → `user.id` | ✓ | Ai nhận |
| `note` | text | — | – | Ghi chú |
