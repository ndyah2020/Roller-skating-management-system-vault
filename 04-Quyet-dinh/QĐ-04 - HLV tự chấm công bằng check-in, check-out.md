---
ma: QĐ-04
ten: "HLV tự chấm công bằng check-in, check-out"
loai: quyết định
ngay: 2026-09-09
trang_thai: Đã chốt
anh_huong:
  - "[[NS - Nhân sự]]"
  - "[[DH - Dạy học]]"
  - "[[LG - Lương và thù lao]]"
tags:
  - quyet-dinh
---

# QĐ-04 - HLV tự chấm công bằng check-in, check-out

> Ngày: 2026-09-09

## Bối cảnh

Cần biết HLV có mặt tại sân đúng giờ không, vừa để tính lương, vừa để phục vụ điểm danh (BR-07 dùng vị trí check-in làm một trong hai điều kiện lọc danh sách).

## Các phương án đã cân nhắc

| Phương án                                                          | Ưu                                     | Nhược                                                        |
| ------------------------------------------------------------------ | -------------------------------------- | ------------------------------------------------------------ |
| A. Admin chấm công thủ công cuối ngày/tuần                         | Admin kiểm soát chặt                   | Thêm việc cho Admin, không có dữ liệu tức thời cho điểm danh |
| B. HLV tự check-in/check-out tại sân, hệ thống so lịch và báo lệch | Tức thời, ít việc cho Admin, đúng NT-5 | Cần cơ chế báo lệch để Admin soát lại khi có bất thường      |

## Quyết định

**Chọn phương án B.** HLV tự check-in / check-out tại sân trên điện thoại; hệ thống so giờ thực tế với lịch và báo lệch (`variance_minutes`) để Admin soát, không cần duyệt từng lần.

## Lý do

Khớp NT-5 (làm được trên điện thoại tại sân) và giảm việc thủ công cho Admin.

## Hệ quả

- [[NS - Nhân sự]] — màn hình S6 (Check-in/check-out)
- [[DH - Dạy học]] — vị trí check-in phục vụ điều kiện lọc điểm danh (BR-07)
- [[LG - Lương và thù lao]] — dữ liệu chấm công là đầu vào tính công
