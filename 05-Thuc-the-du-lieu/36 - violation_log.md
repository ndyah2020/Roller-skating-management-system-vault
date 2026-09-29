---
ten: "Nhật ký vi phạm"
loai: thực thể
bang_db: violation_log
module_chu: NS
so_thu_tu: 36
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Nhật ký vi phạm"
---

# violation_log

> #36 · Nhật ký vi phạm · Module chủ: NS

Một dòng = một lần một HLV vi phạm một [[Loại lỗi vi phạm]] — tự động sinh (đi trễ) hoặc Quản lý/Admin ghi tay (đồng phục, điện thoại…).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV vi phạm |
| `violation_type_id` | uuid | FK → `violation_type.id` | ✓ | Loại lỗi |
| `occurred_at` | timestamptz | — | ✓ | Thời điểm vi phạm (hoặc lúc ghi nhận, nếu ghi tay) |
| `source` | text | — | ✓ | enum: `system` (tự động, chỉ dùng cho `basis = duration`) / `manual` (Quản lý hoặc Admin ghi) |
| `measured_value` | integer | — | ✓ | Giá trị đo được — số phút trễ (`basis = duration`) hoặc số thứ tự lần vi phạm loại này trong kỳ hiện tại (`basis = count`) |
| `penalty_amount` | bigint | — | ✓ | Số tiền phạt — chốt theo [[Mức phạt theo bậc]] tại thời điểm ghi nhận, lưu lại cố định (sau này sửa bậc phạt không làm đổi số đã ghi) |
| `timesheet_id` | uuid | FK → `timesheet.id` | – | Buổi chấm công liên quan — chỉ có khi `source = system` |
| `recorded_by` | uuid | FK → `user.id` | – | Người ghi nhận thủ công — để trống nếu `source = system` |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Index: (`staff_id`, `violation_type_id`, `occurred_at`)
- Index: `timesheet_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]] — BR-34, BR-35, BR-36
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú

- `penalty_amount` chảy vào lương qua [[Dòng cấu thành lương]] bằng cột đa hình sẵn có (`payroll_line.reference_type = 'violation_log'` + `reference_id`) — không thêm cột `payroll_line_id` ở bảng này. Xem bảng tổng hợp cột đa hình ở [[Sơ đồ dữ liệu]].
- Sửa hoặc xoá một dòng đã sinh `payroll_line` thì phải sửa/xoá luôn dòng lương tương ứng — miễn còn trước khi chốt kỳ lương (BR-36).
