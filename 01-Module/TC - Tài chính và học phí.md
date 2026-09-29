---
ma: TC
ten: "Tài chính và học phí"
loai: module
giai_doan: 1
trang_thai: Đã đặc tả
tags:
  - module
---

# TC - Tài chính và học phí

> Giai đoạn 1 · Gói học và thu tiền, gắn liền với việc trừ buổi ở DH.

## Mục đích

Bán gói, thu tiền, công nợ, chi phí và chốt sổ. Gói học của học viên bị trừ buổi qua chính quy trình chốt ngày ở DH, không trừ trực tiếp.

## Chức năng chính

- Bảng giá và gói học: theo buổi · theo khoá · combo
- Gói của từng học viên với số buổi còn lại
- Phiếu thu và hoá đơn: tiền mặt · chuyển khoản · QR
- Công nợ và nhắc đóng học phí
- Giảm giá, hoàn tiền
- Thu khác: bán giày, thuê giày (liên kết BH, TS)
- Chi: lương, mua sắm, marketing
- Báo cáo doanh thu và lợi nhuận
- Chốt sổ theo tháng, khoá sửa số liệu quá khứ

## Màn hình

Không có mã S riêng — thao tác thu/chi nằm trong S4, S5 (module HV) và một phần S3 (chốt ngày ở DH sinh biến động ảnh hưởng tới gói).

## Quy trình liên quan

- P1 — Ghi danh và bán gói
- P4 — Chốt ngày (trừ vào gói)
- P5 — Chốt lương kỳ (sinh khoản chi)
- P7 — Đặt giày cho khách (sinh hoá đơn)

## Quy tắc nghiệp vụ liên quan

- [[QT-03 - Trừ buổi]] — cách một buổi học biến thành số buổi trừ trong gói
- [[QT-06 - Dữ liệu và kế toán]] — BR-30 đến BR-32

## Vai trò liên quan

- [[Admin]] — toàn quyền, không có vai trò Kế toán riêng
- [[Quản lý]] — toàn quyền
- [[Phụ huynh - Học viên]] — xem hoá đơn và số buổi còn lại

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`package` · `student_package` · `invoice` · `invoice_line` · `payment` · `receivable` · `expense` · `accounting_period`

## Quyết định liên quan

- [[QĐ-02 - Không tự động trừ buổi, chốt cuối ngày]]
- [[QĐ-06 - Không có vai trò Kế toán riêng]]

## Ghi chú / câu hỏi mở

- Chưa tích hợp VNPay sandbox — mới ghi nhận là hướng cho thanh toán QR.
