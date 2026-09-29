---
ma: QĐ-11
ten: "Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra"
loai: quyết định
ngay: 2026-09-28
trang_thai: Đã chốt
anh_huong:
  - "[[LG - Lương và thù lao]]"
  - "[[NS - Nhân sự]]"
tags:
  - quyet-dinh
---

# QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra

> Ngày: 2026-09-28 · Sửa lại [[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]]

## Bối cảnh

[[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]] đặt ca lẻ là một khoản **phụ cấp cộng thêm** vào lương giờ (BR-24, BR-25 bản cũ), và hệ thống **tự động suy ra** một cụm là ca lẻ khi cụm đó chỉ có một buổi một giờ. Cách vận hành thực tế cần đơn giản và linh hoạt hơn hai điểm này.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ nguyên QĐ-01 — phụ cấp cộng thêm, tự động suy ra | Không cần sửa gì | Không khớp cách CLB thật sự muốn tính — ca lẻ phải là lương thay thế, không phải cộng thêm; suy tự động cứng nhắc, không cho người xử lý lương linh hoạt |
| B. Ca lẻ là lương thay thế (không cộng thêm giờ), chọn thủ công ca thường/ca lẻ cho mỗi cụm, mặc định ca thường | Đúng ý muốn thực tế, linh hoạt, sửa được khi chọn nhầm | Cần người xử lý lương tự chọn đúng, không có logic tự chặn tự động |

## Quyết định

**Chọn phương án B.**

- Mỗi cụm ca (xem [[QT-04 - Lương]], BR-22) có một lựa chọn thủ công: **ca thường** (mặc định) hoặc **ca lẻ**.
- **Ca thường** → lương = đơn giá theo giờ ([[Mức lương theo giờ]]) × giờ công của cụm.
- **Ca lẻ** → lương = mức cố định, mặc định **100.000đ** (cấu hình được ở [[Quy tắc tính công]]) — **thay thế hoàn toàn** cách tính theo giờ, không cộng thêm nữa.
- Chọn nhầm thì người chọn tự sửa lại được, hoặc Admin sửa — miễn còn trước khi chốt kỳ lương ([[Bảng công theo kỳ]] còn ở trạng thái `draft`).

## Lý do

Ca lẻ giờ được hiểu là "một cách tính lương khác cho ca này", không phải "thêm một khoản ngoài lương giờ" — flat 100.000đ dễ hiểu, dễ giải thích hơn cộng dồn hai khoản. Chọn thủ công thay vì suy tự động cũng linh hoạt hơn: người xếp lịch/chấm công biết rõ thực tế hôm đó hơn một quy tắc cứng ("đúng 1 buổi 1 giờ mới tính ca lẻ").

## Hệ quả

- [[QT-04 - Lương]] — sửa lại BR-24 (chọn thủ công, không tự suy) và BR-25 (thay thế, không cộng thêm).
- [[Quy tắc tính công]] (`pay_rule`) và [[Dòng cấu thành lương]] (`payroll_line`) — đổi tên giá trị enum `odd_shift_allowance` → `odd_shift` cho khớp (không còn là "phụ cấp").
- [[Mức lương theo giờ]] (`staff_pay_rate`) — đơn giá theo giờ chỉ áp dụng cho cụm chọn **ca thường**.
- [[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]] vẫn đúng phần "số tiền cố định, cấu hình được" — chỉ sai phần "phụ cấp cộng thêm" và "tự động suy ra". Xem mục Cập nhật ở note đó.
