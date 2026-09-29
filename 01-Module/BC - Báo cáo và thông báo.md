---
ma: BC
ten: "Báo cáo và thông báo"
loai: module
giai_doan: 3
trang_thai: Đã đặc tả
tags:
  - module
---

# BC - Báo cáo và thông báo

> Giai đoạn 3 · Đọc dữ liệu từ các module khác — không phải nguồn dữ liệu gốc.

## Mục đích

Dashboard, danh sách việc cần xử lý, và thông báo chủ động qua Zalo/SMS/email.

## Chức năng chính

- Dashboard: học viên đang học · buổi hôm nay · doanh thu tháng · công nợ
- Danh sách việc cần xử lý: gói sắp hết buổi · tài sản chưa thu hồi · điểm danh chưa chốt cuối ngày
- Thông báo qua Zalo, SMS, email: nhắc buổi học · nhắc đóng tiền · báo nghỉ do mưa · kết quả lên cấp
 
> Phần báo cáo đọc trực tiếp từ dữ liệu các module khác, không cần bảng riêng ngoài mẫu thông báo và nhật ký gửi.

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S11 | Dashboard | Admin | Việc cần xử lý hiện ngay đầu trang, không phải tìm |

## Quy trình liên quan

Đọc dữ liệu ra từ P1–P7, không sinh ra quy trình riêng.

## Quy tắc nghiệp vụ liên quan

Không có quy tắc riêng.

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — cảnh báo liên quan tới mình
- [[Phụ huynh - Học viên]] — thông báo gửi cho mình

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`notification_template` · `notification_log` · `alert`

## Quyết định liên quan

Không có quyết định riêng.

## Ghi chú / câu hỏi mở

Không có câu hỏi mở.
