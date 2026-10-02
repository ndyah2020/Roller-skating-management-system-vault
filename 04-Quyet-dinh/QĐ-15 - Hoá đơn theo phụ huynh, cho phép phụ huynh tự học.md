---
ma: QĐ-15
ten: "Hoá đơn theo phụ huynh, cho phép phụ huynh tự học"
loai: quyết định
ngay: 2026-10-02
trang_thai: Đã chốt
anh_huong:
  - "[[TC - Tài chính và học phí]]"
  - "[[HV - Học viên và phụ huynh]]"
tags:
  - quyet-dinh
---

# QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học

> Ngày: 2026-10-02

## Bối cảnh

Thiết kế ban đầu: `invoice.student_id` bắt buộc — mỗi hoá đơn chỉ gắn 1 học viên, `invoice_line` không có cột riêng cho người học. Khi rà lại, phát hiện 2 vấn đề:

1. Một phụ huynh có 2 con, đăng ký/mua gói cho cả 2 cùng lúc → phải tạo 2 hoá đơn riêng (vì `student_id` bắt buộc, không gộp được), dù thực tế chỉ là 1 lần giao dịch. `payment.invoice_id` cũng chỉ gắn 1 hoá đơn, nên 1 lần phụ huynh đưa tiền mặt/chuyển khoản cho cả 2 hoá đơn đó cũng không có cách ghi đúng — phải tách thành 2 dòng `payment` giả.
2. Người dùng xác nhận thêm: phụ huynh không chỉ đăng ký cho con, mà **chính phụ huynh cũng có thể tự đăng ký học**. Bảng `student` bản cũ định nghĩa cứng là "trẻ học patin", không có chỗ cho trường hợp này.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ `invoice` theo học viên, thêm bảng/cơ chế cho 1 lần thu áp vào nhiều hoá đơn | Mỗi học viên vẫn có hoá đơn riêng, dễ tra lịch sử từng con | Thêm khái niệm "phiếu thu" tách khỏi "hoá đơn", không khớp với cách phụ huynh thực nhận (1 tờ hoá đơn cho 1 lần đóng tiền) |
| B. Hoá đơn theo phụ huynh (`guardian_id` bắt buộc), chuyển `student_id` xuống `invoice_line` | 1 hoá đơn = đúng 1 giao dịch/1 tờ giấy thực tế, dù gồm bao nhiêu người học; `payment` không cần đổi gì | Mất cột `student_id` trực tiếp trên `invoice` — tra "hoá đơn của học viên X" phải qua `invoice_line` |

Riêng vấn đề "phụ huynh tự học": cân nhắc tạo bảng riêng cho người học là người lớn, nhưng chọn cách tái dùng bảng `student` sẵn có (xem Quyết định) để không phải đổi `enrollment`, `session`, `student_package`, `credit_transaction`... — tất cả vẫn hoạt động nguyên vẹn vì vẫn trỏ qua `student_id` như cũ.

## Quyết định

**Chọn phương án B.** `invoice.guardian_id` đổi thành bắt buộc (chủ thể hoá đơn), bỏ `invoice.student_id`. Thêm `invoice_line.student_id` (cho phép trống) để mỗi dòng tự ghi rõ của người học nào.

Với phụ huynh tự học: thêm cột `student.guardian_self_id` (FK → `guardian.id`, cho phép trống) — khi có giá trị, dòng `student` đó chính là hồ sơ học của phụ huynh đó, không phải con của họ. Không tạo bảng mới, không tạo dòng tự-liên-kết ở `student_guardian`.

## Lý do

Một hoá đơn nên phản ánh đúng **một giao dịch/một tờ giấy thực tế** đưa cho phụ huynh — có thể gồm nhiều dòng cho nhiều người học khác nhau, thay vì buộc phải tách thành nhiều hoá đơn chỉ vì có nhiều người học trong 1 lần mua. Nhờ vậy `payment` không cần sửa gì — 1 lần thu vẫn áp đúng vào 1 hoá đơn, vì hoá đơn đã gộp sẵn mọi người học của giao dịch đó.

Tái dùng `student` cho cả trường hợp phụ huynh tự học (qua `guardian_self_id`) thay vì tạo cấu trúc riêng giữ cho toàn bộ chuỗi `enrollment` → `session` → `attendance` → `student_package` → `credit_transaction` không phải đổi gì — "người học" vốn đã là khái niệm đủ rộng ở các bảng đó, chỉ riêng mô tả của bảng `student` cần nói rõ lại.

## Hệ quả

- `invoice` — bỏ `student_id`, `guardian_id` đổi bắt buộc, đổi index.
- `invoice_line` — thêm `student_id` (cho phép trống), thêm index.
- `student` — mở rộng mô tả thành "người học" (không chỉ trẻ), thêm cột `guardian_self_id`.
- `receivable` — bỏ `student_id` (trùng/hết rõ nghĩa), theo dõi theo `invoice_id`.
- `student_guardian` — không đổi cột, chỉ ghi rõ: không dùng cho trường hợp phụ huynh tự học.
- `payment` — không đổi gì.
- Module `TC`, `HV` — cập nhật mô tả liên quan.
- Nếu sau này cần phân biệt "học viên trẻ" và "học viên là phụ huynh" ở báo cáo/màn hình, lọc qua `guardian_self_id IS NULL`/`IS NOT NULL`, không cần join thêm bảng nào khác.
