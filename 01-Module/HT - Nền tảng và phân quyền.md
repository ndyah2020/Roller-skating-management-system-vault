---
ma: HT
ten: "Nền tảng và phân quyền"
loai: module
giai_doan: 1
trang_thai: Đã đặc tả
tags:
  - module
---

# HT - Nền tảng và phân quyền

> Giai đoạn 1 · Khung chung — không có nó thì không chạy gì được.

## Mục đích

Ai đăng nhập được, thấy được gì, và sân nào đang dùng. Mọi module khác dựa vào tài khoản, quyền và danh mục khai báo ở đây.

## Chức năng chính

- Tài khoản, phiên đăng nhập, đổi và quên mật khẩu
- Phân quyền theo vai trò: Admin · HLV · Phụ huynh
- Khai báo sân dạy học: tên, địa chỉ, khung giờ dùng được
- Danh mục dùng chung (trình độ, loại tài sản, nguồn khách, phương thức thanh toán…)
- Nhật ký thao tác — bắt buộc với mọi thay đổi về tiền và tài sản

## Màn hình

Không có màn hình riêng trong S1–S11 — đăng nhập và phân quyền nằm bên trong mọi màn hình khác.

## Quy trình liên quan

Là nền cho toàn bộ P1–P7, không phải chủ của quy trình nào.

## Quy tắc nghiệp vụ liên quan

- [[QT-06 - Dữ liệu và kế toán]] — BR-30, BR-31 (ghi vết, không xoá cứng) áp dụng và quản lý chủ yếu ở đây.

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — không có quyền truy cập module này
- [[HLV]] · [[Phụ huynh - Học viên]] — chỉ xem hồ sơ mình

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`user` · `role` · `permission` · `venue` · `venue_time_slot` · `category` · `audit_log`

Ghi chú: sân **không** cần cột sức chứa cố định — sức chứa suy ra từ số HLV được phân vào khung giờ đó, xem BR-03 ở [[QT-01 - Lớp và lịch]].

## Quyết định liên quan

Không có quyết định riêng — theo 3 quy tắc xuyên suốt ở [[QT-06 - Dữ liệu và kế toán]].

## Ghi chú / câu hỏi mở

- [ ] ❓ Mức chi tiết của `permission`: theo màn hình, hay theo từng bản ghi? — để ngỏ tới khi thiết kế database.
