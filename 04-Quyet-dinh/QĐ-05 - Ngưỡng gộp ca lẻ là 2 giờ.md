---
ma: QĐ-05
ten: "Ngưỡng gộp ca lẻ là 2 giờ"
loai: quyết định
ngay: 2026-09-23
trang_thai: Đã chốt
anh_huong:
  - "[[LG - Lương và thù lao]]"
  - "[[NS - Nhân sự]]"
tags:
  - quyet-dinh
---

# QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ

> Ngày: 2026-09-23 · Trả lời cho câu hỏi mở về ngưỡng N ở BR-22

## Bối cảnh

BR-22 cần một ngưỡng khoảng cách cụ thể để biết hai buổi trong ngày của một HLV có tính là "cùng cụm" hay không. Ví dụ gốc chỉ xác nhận: cách 1 giờ là cùng cụm, cách 7 giờ là khác cụm — chưa rõ khoảng 2–3 giờ tính sao.

## Các phương án đã cân nhắc

| Phương án               | Ưu                                                                   | Nhược                                         |
| ----------------------- | -------------------------------------------------------------------- | --------------------------------------------- |
| A. 1 giờ                | An toàn, ít gộp nhầm                                                 | Có thể tách cụm quá sớm với lịch dạy sát nhau |
| B. 2 giờ, cấu hình được | Nằm giữa hai mốc đã xác nhận (1h và 7h), sửa được không cần đổi code | Vẫn là một lựa chọn tạm, cần theo dõi thực tế |
|                         |                                                                      |                                               |

## Quyết định

**Chọn phương án B** — ngưỡng N = 2 giờ, đặt thành tham số cấu hình được (không hard-code).

## Lý do

Nằm giữa khoảng đã được xác nhận qua ví dụ thực tế, và để cấu hình được nên nếu sai thì sửa số, không cần sửa code.

## Hệ quả

- [[LG - Lương và thù lao]] — cách gộp cụm ca khi tính công
- [[NS - Nhân sự]]
- Xem thêm: [[QT-04 - Lương]]
