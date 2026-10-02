---
ma: HV
ten: "Học viên và phụ huynh"
loai: module
giai_doan: 1
trang_thai: Đã đặc tả
tags:
  - module
---

# HV - Học viên và phụ huynh

> Giai đoạn 1 · Dữ liệu gốc của mọi việc còn lại.

## Mục đích

Hồ sơ học viên, phụ huynh/người giám hộ, và ghi danh nơi bắt đầu của quy trình P1 (ghi danh và bán gói).

## Chức năng chính

- Hồ sơ học viên: ảnh, ngày sinh, size chân, size bảo hộ, bài đang học, trạng thái
- Hồ sơ phụ huynh; một phụ huynh nhiều con, một học viên nhiều người giám hộ. Phụ huynh cũng có thể tự đăng ký học cho chính mình, không chỉ cho con — xem [[Học viên]]
- Ghi chú y tế và an toàn *(tuỳ chọn)*
- Ghi danh theo sân và giờ — một học viên học được ở nhiều sân khác nhau, nhưng **không đăng ký hai sân trong cùng một ngày**
- Thông báo về số buổi còn lại và tiến độ hiện tại

## Màn hình

| Mã  | Màn hình       | Tác nhân         | Mục tiêu thao tác                                                                                                       |
| --- | -------------- | ---------------- | ----------------------------------------------------------------------------------------------------------------------- |
| S4  | Hồ sơ học viên | Admin, Phụ huynh | Một màn hình thấy đủ: buổi còn lại, bài đang học, lịch sắp tới, công nợ                                                 |
| S5  | Ghi danh nhanh | Admin            | Nhập học viên mới và xếp vào ô lịch trong cùng một luồng, không nhảy màn hình *(chạm cả TC để bán gói, DH để xếp lịch)* |


## Quy trình liên quan

- P1 — Ghi danh và bán gói

## Quy tắc nghiệp vụ liên quan

- [[QT-01 - Lớp và lịch]] — BR-04 (không đăng ký hai sân cùng ngày)

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — xem toàn bộ học viên của buổi mình được phân
- [[Phụ huynh - Học viên]] — hồ sơ con mình

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`student` · `guardian` · `student_guardian` · `medical_note` · `enrollment`

## Quyết định liên quan

- [[QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học]]

## Ghi chú / câu hỏi mở

- [ ] ❓ `student.preferred_staff_id` (HLV mong muốn) — chỉ là gợi ý mềm, có thể bỏ nếu thấy thừa. Thuộc phần thiết kế database.
