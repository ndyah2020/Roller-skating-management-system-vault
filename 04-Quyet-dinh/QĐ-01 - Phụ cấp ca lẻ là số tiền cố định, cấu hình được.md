---
ma: QĐ-01
ten: "Phụ cấp ca lẻ là số tiền cố định, cấu hình được"
loai: quyết định
ngay: 2026-09-09
trang_thai: Đã chốt
anh_huong:
  - "[[LG - Lương và thù lao]]"
  - "[[NS - Nhân sự]]"
tags:
  - quyet-dinh
---

# QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được

> Ngày: 2026-09-09

## Bối cảnh

Cần cách tính thêm cho HLV chỉ dạy một ca ngắn (một tiếng tại một sân) rồi về, để công bằng so với người dạy nhiều buổi liên tục trong cụm dài.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Không phụ cấp gì thêm | Đơn giản | Không công bằng với ca lẻ |
| B. Phụ cấp theo % lương buổi | Tự co giãn theo lương | Khó giải thích, khó đoán |
| C. Phụ cấp cố định, cấu hình được | Dễ tính, dễ hiểu, sửa được không cần đổi code | Phải chọn một mức cụ thể ban đầu |

## Quyết định

**Chọn phương án C** — mỗi ca lẻ cộng thêm một số tiền cố định, cấu hình được (ví dụ 100.000đ). Ca lẻ = HLV chỉ dạy một tiếng tại một sân rồi về (BR-24).

## Lý do

Dễ tính, dễ hiểu, và tách biệt khỏi giờ công — giờ công của ca lẻ vẫn tính bình thường, phụ cấp chỉ là khoản cộng thêm (BR-25).

## Hệ quả

- [[LG - Lương và thù lao]] — quy tắc tính công `pay_rule` loại `odd_shift_allowance` *(nay đổi tên thành `odd_shift`, xem Cập nhật bên dưới)*
- Xem thêm: [[QT-04 - Lương]], [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]]

## Cập nhật (2026-09-28)

[[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]] sửa lại 2 điểm ở quyết định này:

1. Ca lẻ **không còn là phụ cấp cộng thêm** vào lương giờ — là lương **thay thế** hoàn toàn cách tính theo giờ cho cụm đó.
2. **Không còn tự động suy ra** ca lẻ từ số buổi trong cụm — người xử lý lương **chọn thủ công** ca thường/ca lẻ cho mỗi cụm, mặc định ca thường, sửa được khi chọn nhầm.

Phần còn đúng: mức 100.000đ là **số tiền cố định, cấu hình được** (không hard-code) — chỉ đổi cách nó được áp dụng.
