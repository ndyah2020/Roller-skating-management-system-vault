---
ten: "Bảng công theo kỳ"
loai: thực thể
bang_db: payroll
module_chu: LG
trang_thai: Nháp
tags:
  - thuc-the
---

# Bảng công theo kỳ

> Bảng: `payroll` · Module chủ: LG

Một dòng = tổng hợp công và lương của một HLV trong một kỳ lương. Chi tiết từng khoản nằm ở [[Dòng cấu thành lương]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `payroll_period_id` | uuid | FK → `payroll_period.id` | ✓ | Kỳ lương |
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV |
| `total_hours` | numeric(6,2) | — | ✓ | Tổng giờ công, tính theo cụm ca (BR-22, BR-23) |
| `total_sessions` | integer | — | ✓ | Tổng số buổi đã dạy |
| `odd_shift_count` | integer | — | ✓ | Số ca lẻ trong kỳ (BR-24) |
| `gross_amount` | bigint | — | ✓ | Lương trước khấu trừ |
| `deduction_amount` | bigint | — | ✓ | Tổng khấu trừ |
| `net_amount` | bigint | — | ✓ | Lương thực nhận = `gross_amount` − `deduction_amount` |
| `status` | text | — | ✓ | enum: `draft` / `finalized` / `paid` |

## Ràng buộc & chỉ mục

- Unique: (`payroll_period_id`, `staff_id`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]] — toàn bộ BR-21 đến BR-25, BR-34 đến BR-36
- [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]]
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú

- `deduction_amount` là tổng các dòng `payroll_line.line_type = deduction` trong kỳ — gồm cả khấu trừ vi phạm (đi trễ, đồng phục, dùng điện thoại… — xem [[Nhật ký vi phạm]], BR-34 đến BR-36) và khấu trừ do làm hỏng/mất tài sản.
