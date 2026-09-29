---
ma: DH
ten: "Dạy học"
loai: module
giai_doan: 1
trang_thai: Đã đặc tả
tags:
  - module
---

# DH - Dạy học

> Giai đoạn 1 · Trục chính của hệ thống: lịch, điểm danh, trừ buổi.

## Mục đích

Chương trình, lớp, lịch, điểm danh, và việc trừ buổi. `session` (buổi học) là bảng trục điểm danh, chấm công HLV, trừ buổi trong gói đều móc vào nó.

## Chức năng chính

- Thiết kế chương trình theo trình độ (level) và bài học (lesson) — các mốc kỹ năng cố định theo thứ tự (chạy thắng, đi cốc, đi lùi…); mỗi học viên đang ở một bài (`current_lesson_id`)
- Mở lớp: **1-1** (một HLV một học viên) · **nhóm** (tối đa 4 học viên/HLV) · **nửa 1-1 nửa nhóm** khi thiếu HLV. Thời lượng chuẩn 1 tiếng
- Xếp lịch tuần định kỳ, sửa tự do trong tuần trước giờ học (P2)
- Cảnh báo khi xếp lịch: thiếu HLV theo tỉ lệ 1:4 · HLV trùng giờ ở hai sân · học viên đăng ký hai sân cùng ngày
- Điểm danh tại sân, chống tích trùng (P3)
- Trừ buổi theo điểm danh, chốt cuối ngày, có ngoại lệ (P4)
- Ghi tiến độ bài học, nhận xét, ảnh/video
- Huỷ buổi do mưa, dời lịch
## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S1 | Lịch tuần theo sân | Admin | Kéo thả một buổi sang giờ khác. Ô lịch tự hiện badge đỏ khi thiếu HLV |
| S2 | Điểm danh tại sân | HLV | 1 chạm mỗi học viên. Danh sách tự mở đúng sân/khung giờ theo check-in |
| S3 | Chốt ngày | Admin | 1 nút duyệt tất cả, sửa lẻ từng dòng khi cần |
 
## Quy trình liên quan

- P2 — Xếp lịch tuần
- P3 — Buổi dạy tại sân
- P4 — Chốt ngày

Chi tiết sơ đồ ba quy trình: xem note Tổng quan hệ thống.
 
## Quy tắc nghiệp vụ liên quan

- [[QT-01 - Lớp và lịch]] — BR-01 đến BR-06 
- [[QT-02 - Điểm danh]] — BR-07 đến BR-12
- [[QT-03 - Trừ buổi]] — BR-13 đến BR-20 và BR-33

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — xem lịch mình · điểm danh buổi ở sân mình đang có mặt · ghi tiến độ
- [[Phụ huynh - Học viên]] — xem lịch và tiến độ của con

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`course` · `level` · `lesson` · `student_lesson` · `session` · `session_coach` · `attendance` · `credit_transaction`

## Quyết định liên quan

- [[QĐ-07 - Nửa buổi 1-1 chuyển nhóm, CLB bù phần thiếu HLV]]

## Ghi chú / câu hỏi mở

- [ ] ❓ `half_one_on_one_half_group` là một `course` riêng hay chỉ là giá trị thực tế dạy? — thuộc phần thiết kế database, để ngỏ.
