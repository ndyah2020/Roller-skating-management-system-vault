---
ten: "Phiếu lương"
loai: thực thể
bang_db: payslip
module_chu: LG
trang_thai: Nháp
tags:
  - thuc-the
---

# Phiếu lương

> Bảng: `payslip` · Module chủ: LG

Quan hệ 1–1 với [[Bảng công theo kỳ]]. Khi trạng thái chuyển `paid`, sinh một dòng ở [[Khoản chi]] bên TC.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `payroll_id` | uuid | FK → `payroll.id`, unique | ✓ | Bảng công tương ứng |
| `issued_at` | timestamptz | — | ✓ | Lúc phát hành phiếu |
| `paid_at` | timestamptz | — | – | Lúc đã chi trả |
| `payment_method` | text | — | – | Cách trả (tiền mặt/chuyển khoản) |
| `expense_id` | uuid | FK → `expense.id` | – | Khoản chi sinh ra khi đã trả |
| `status` | text | — | ✓ | enum: `issued` / `paid` |

## Ràng buộc & chỉ mục

- Unique: `payroll_id`
