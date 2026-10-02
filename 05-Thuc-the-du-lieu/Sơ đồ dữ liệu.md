---
loai: ghi chú
tags:
  - thuc-the
---

# Sơ đồ dữ liệu — tổng quan 64 bảng

> Khung nháp để bạn xem xét và chỉnh sửa dần. Toàn bộ tên bảng/cột dưới đây bằng tiếng Anh `snake_case`, mô tả bằng tiếng Việt — theo đúng quy ước ở [[Quy ước đặt tên]].

## Quy ước kiểu dữ liệu (áp dụng cho mọi bảng dưới đây)

Vì chưa chọn stack cụ thể, các kiểu ghi dưới đây theo chuẩn gần với PostgreSQL — đổi tên kiểu cho khớp hệ CSDL bạn chọn thì các cột/khoá vẫn giữ nguyên ý nghĩa.

| Chủ đề | Quy ước đã chọn | Vì sao |
|---|---|---|
| Khoá chính | `uuid`, mọi bảng đều có cột `id` làm PK | Sinh được ở client (điện thoại HLV tại sân, có thể mất mạng), không lộ số thứ tự. Muốn đổi sang số tự tăng thì đổi ở đây, áp dụng lại toàn bộ |
| Tiền (VND) | `bigint` | VND không dùng số lẻ dưới đồng, số nguyên là đủ và tránh sai số thập phân |
| Số buổi học | `numeric(5,2)` | Phải cho phép nửa buổi (0.5) theo BR-18 |
| Thời gian | `timestamptz` cho mốc giờ chính xác · `date` cho ngày · `time` cho khung giờ lặp lại (ví dụ `venue_time_slot`) | Lưu UTC, quy đổi giờ hiển thị ở tầng ứng dụng |
| Giá trị enum (`status`, `role`…) | `text` + kiểm tra hợp lệ ở tầng ứng dụng (hoặc `CHECK` nếu CSDL hỗ trợ) | Thêm/sửa giá trị không cần đổi schema |
| Nội dung tự do dài (JSON, ảnh trước/sau…) | `jsonb` khi cần lưu cấu trúc (ví dụ `audit_log.old_value`) | — |

**Cột chung của mọi bảng vận hành** (không lặp lại ở từng bảng bên dưới — xem [[Quy ước đặt tên]]):

`id` (PK) · `created_at` · `updated_at` · `created_by` (FK → `user.id`) · `updated_by` (FK → `user.id`) · `is_active`

Ngoại lệ: các bảng **log bất biến** (`audit_log`, `notification_log`) không có `updated_at` / `updated_by` / `is_active` vì không sửa sau khi ghi — ghi rõ trong note từng bảng.

### Cột đa hình (polymorphic) — không có khoá ngoại cứng ở tầng CSDL

Vài bảng dùng cặp cột `xxx_type` + `xxx_id` để một cột `id` có thể trỏ tới nhiều bảng khác nhau tuỳ giá trị `xxx_type`. CSDL **không kiểm tra được** kiểu FK thông thường cho các cột này — phải tự kiểm tra tính đúng đắn ở tầng ứng dụng (hoặc tách bảng riêng nếu sau này thấy rủi ro sai dữ liệu quá cao). Danh sách để bạn để ý khi thiết kế:

| Bảng | Cặp cột đa hình | Trỏ tới (tuỳ `_type`) |
|---|---|---|
| `custody_log` | `item_type` + `item_id` | `asset.id` hoặc `stock_item.id` |
| `custody_log` | `holder_type` + `holder_id` | `staff.id` (nếu `staff`) · để trống nếu `warehouse` · `student.id` (nếu `student`) |
| `invoice_line` | `item_type` + `item_id` | `package.id` / `product.id` / `equipment_rental.id` |
| `expense` | `reference_type` + `reference_id` | `payslip.id` / `asset_maintenance.id` / `purchase_batch.id` / `campaign.id` |
| `payroll_line` | `reference_type` + `reference_id` | ví dụ `timesheet.id`, `asset_maintenance.id`, `violation_log.id` |
| `referral` | `referrer_type` + `referrer_id` | `guardian.id` hoặc `staff.id` |
| `notification_log` | `recipient_type` + `recipient_id` | `guardian.id` / `staff.id` / `user.id` |
| `alert` | `reference_type` + `reference_id` | tuỳ loại cảnh báo, ví dụ `student_package.id`, `custody_log.id`, `session.id` |

## Toàn bộ 64 bảng theo module

```dataview
TABLE WITHOUT ID
  so_thu_tu AS "#",
  bang_db AS "Bảng",
  link(file.link, default(ten, file.name)) AS "Thực thể",
  module_chu AS "Module",
  trang_thai AS "Schema"
FROM #thuc-the AND -"05-Thuc-the-du-lieu/Sơ đồ dữ liệu" AND -"05-Thuc-the-du-lieu/_Bắt đầu ở đây"
SORT so_thu_tu ASC
```

Số `#` chính là số thứ tự trong tên file (`NN - tên_bảng_tiếng_anh.md`) — sắp theo module (HT→HV→DH→TC→NS→LG→TS→BH→MK→BC), trong mỗi module bảng gốc (không phụ thuộc bảng khác cùng module) đứng trước, bảng phụ thuộc đứng sau. Đây là thứ tự nên đọc theo khi rà lại toàn bộ schema.

### HT — Nền tảng và phân quyền (7 bảng)

`user` · `role` · `permission` · `role_permission` · `venue` · `venue_time_slot` · `audit_log`

### HV — Học viên và phụ huynh (5 bảng)

`student` · `guardian` · `student_guardian` · `medical_note` · `enrollment`

### DH — Dạy học (8 bảng)

`course` · `level` · `lesson` · `student_lesson` · `session` · `session_coach` · `attendance` · `credit_transaction`

### TC — Tài chính và học phí (8 bảng)

`package` · `student_package` · `invoice` · `invoice_line` · `payment` · `receivable` · `expense` · `accounting_period`

### NS — Nhân sự (8 bảng)

`staff` · `staff_availability` · `staff_pay_rate` · `staff_review` · `timesheet` · `violation_type` · `violation_penalty_tier` · `violation_log`

### LG — Lương và thù lao (5 bảng)

`pay_rule` · `payroll_period` · `payroll` · `payroll_line` · `payslip`

### TS — Tài sản và dụng cụ (8 bảng)

`asset_type` · `asset` · `custody_log` · `staff_supply` · `asset_maintenance` · `equipment_rental` · `stock_count` · `stock_count_line`

### BH — Bán hàng và đặt giày (8 bảng)

`product` · `supplier` · `customer_order` · `purchase_batch` · `goods_receipt` · `goods_receipt_line` · `stock_item` · `warranty_return`

### MK — Marketing và tuyển sinh (4 bảng)

`lead` · `trial_session` · `referral` · `campaign`

### BC — Báo cáo và thông báo (3 bảng)

`notification_template` · `notification_log` · `alert`

## Những chỗ đã tự quyết khi viết khung này — bạn xem lại

| Chỗ | Đã chọn | Vì sao |
|---|---|---|
| `student.current_level_id` (bản cũ) | **Bỏ**, chỉ giữ `student.current_lesson_id` | Suy ra trình độ qua `lesson.level_id`, tránh lưu trùng hai nơi dễ lệch nhau |
| `attendance.check_in_at` (bản cũ) | **Bỏ**, chỉ giữ `marked_at` | Trùng ý nghĩa với `marked_at`; check-in thật của HLV đã có ở `timesheet.check_in_at` |
| `staff_review.reviewer_staff_id` (bản cũ) | Đổi thành `reviewer_by` → FK `user.id` | Người đánh giá thường là Admin — Admin không có hồ sơ `staff` |
| `marked_by`, `confirmed_by`, `received_by`… | Trỏ tới `user.id` (không phải `staff.id`) | Đúng quy ước "cột `_by` trỏ tới người bấm nút = `user.id`" — cần thì join tiếp `user.staff_id` |
| `invoice.student_id` (bản cũ) | **Bỏ**, chuyển `student_id` xuống `invoice_line`; `invoice.guardian_id` đổi bắt buộc | 1 hoá đơn cần gộp được nhiều người học (phụ huynh đăng ký nhiều con cùng lúc) |
| Phụ huynh tự học | Thêm `student.guardian_self_id`, tái dùng bảng `student` | Không cần tạo cấu trúc riêng, toàn bộ `enrollment`/`session`/`student_package` không đổi |

## Câu hỏi mở (đã mang từ phần đặc tả nghiệp vụ qua, cần chốt trước khi code)

- [ ] ❓ `skill` gộp hẳn vào `lesson`, `level` giữ làm nhóm bài — xem [[Bài học]]
- [ ] ❓ HLV của một buổi: bảng riêng `session_coach` hay một cột `coach_id`? — đã chọn bảng riêng, xem [[HLV được phân cho buổi]]
- [ ] ❓ `half_one_on_one_half_group` là một `course` riêng, hay chỉ là giá trị của `actual_course_id`? — đã chọn `course` riêng, xem [[Loại hình khoá học]]
- [ ] ❓ Mức chi tiết của `permission`: theo màn hình, hay theo từng bản ghi? — xem [[Phân quyền]]
