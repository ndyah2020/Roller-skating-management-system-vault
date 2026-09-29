---
ten: "Cấp phát không thu hồi"
loai: thực thể
bang_db: staff_supply
module_chu: TS
trang_thai: Nháp
tags:
  - thuc-the
---

# Cấp phát không thu hồi

> Bảng: `staff_supply` · Module chủ: TS

Đồ cấp cho HLV dùng luôn, không cần trả lại (balo, cốc, bánh xe…) — khác với [[Sổ giao–nhận]] (đồ phải trả).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV nhận |
| `item_name` | text | — | ✓ | Tên món |
| `quantity` | integer | — | ✓ | Số lượng — mặc định 1 |
| `unit_price` | bigint | — | – | Đơn giá |
| `amount` | bigint | — | ✓ | Thành tiền |
| `issued_at` | date | — | ✓ | Ngày cấp |
| `expense_id` | uuid | FK → `expense.id` | – | Khoản chi sinh ra |
| `note` | text | — | – | Ghi chú |
