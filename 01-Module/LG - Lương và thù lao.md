---
ma: LG
ten: "Lương và thù lao"
loai: module
giai_doan: 2
trang_thai: Đã đặc tả
tags:
  - module
---

# LG - Lương và thù lao

> Giai đoạn 2 · Biến chấm công (NS) và buổi dạy (DH) thành tiền lương.

## Mục đích

Tính lương HLV theo nhiều cách song song, tính ca lẻ theo lựa chọn thủ công, khấu trừ vi phạm, và sinh phiếu lương/khoản chi.

## Chức năng chính

- Kỳ trả lương theo tháng hoặc theo tuần
- Cách tính: theo giờ (full-time/part-time) · theo buổi · lương cứng cộng thưởng
- Bảng công tổng hợp từ chấm công ([[NS - Nhân sự]]) và buổi học ([[DH - Dạy học]])
- **Ca lẻ:** mỗi cụm ca được chọn thủ công là ca thường hoặc ca lẻ (mặc định ca thường); chọn ca lẻ thì lương của cụm đó là một mức cố định, cấu hình được thay thế hoàn toàn cách tính theo giờ, không cộng thêm. Xem [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]]
- **Khấu trừ:** đi trễ (tự động) và các lỗi khác (ghi tay) — dùng khung [[Nhật ký vi phạm]] chung, phạt tăng dần theo bậc; cộng với khoản khấu trừ do làm hỏng hoặc mất tài sản (liên kết [[TS - Tài sản và dụng cụ]]). Xem [[QT-04 - Lương]] (BR-34 đến BR-36) và [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]
- Phiếu lương và trạng thái đã chi → sinh một khoản chi bên [[TC - Tài chính và học phí]]

Cách tính cụm ca và ví dụ kiểm chứng: xem [[QT-04 - Lương]].

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S7 | Bảng lương kỳ | Admin | Xem được từng dòng cấu thành, sửa được trước khi chốt |

## Quy trình liên quan

- P5 — Chốt lương kỳ

## Quy tắc nghiệp vụ liên quan

- [[QT-04 - Lương]] — BR-21 đến BR-25, BR-34 đến BR-36

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — xem phiếu lương của mình

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`pay_rule` · `payroll_period` · `payroll` · `payroll_line` · `payslip`

## Quyết định liên quan

- [[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]]
- [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]]
- [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]]
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú / câu hỏi mở

Không còn câu hỏi mở ngoài phần thiết kế database.
