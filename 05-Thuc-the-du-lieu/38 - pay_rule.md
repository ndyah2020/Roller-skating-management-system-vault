---
ten: "Quy tắc tính công"
loai: thực thể
bang_db: pay_rule
module_chu: LG
so_thu_tu: 38
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Quy tắc tính công"
---

# pay_rule

> #38 · Quy tắc tính công · Module chủ: LG

Một dòng = một cách tính lương áp dụng cho một HLV (hoặc chung cho mọi HLV nếu `staff_id` để trống, ví dụ mức phụ cấp ca lẻ chung toàn CLB).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | – | Để trống nếu là quy tắc chung |
| `rule_type` | text | — | ✓ | enum: `hourly` / `per_session` / `base_salary` / `odd_shift` / `bonus` / `deduction` |
| `amount` | bigint | — | ✓ | Số tiền hoặc đơn giá, VND |
| `unit` | text | — | – | Đơn vị: giờ / buổi / ca / tháng |
| `effective_from` | date | — | ✓ | Áp dụng từ ngày |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`staff_id`, `rule_type`, `effective_from`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]]
- [[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]]

## Ghi chú

- Đã bỏ `rule_type = per_student` (tính theo đầu học viên) so với bản cũ.
- `odd_shift` (trước là `odd_shift_allowance`) không còn là phụ cấp cộng thêm — là mức lương **thay thế hoàn toàn** cho cụm ca được chọn là ca lẻ. Xem [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]].
