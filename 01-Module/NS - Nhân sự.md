---
ma: NS
ten: "Nhân sự"
loai: module
giai_doan: 2
trang_thai: Đã đặc tả
tags:
  - module
---

# NS - Nhân sự

> Giai đoạn 2 · Hồ sơ HLV, năng lực, lịch rảnh, chấm công theo giờ.

## Mục đích

Quản lý HLV: rảnh lúc nào, chấm công thực tế tại sân, và các lỗi vi phạm bị khấu trừ lương — là đầu vào để tính lương ở LG.

## Chức năng chính

- Thông tin cá nhân HLV
- HLV đăng ký lịch rảnh; Admin phân ca theo sân và giờ
- **Chấm công: HLV tự check-in / check-out tại sân**; hệ thống so giờ thực tế với lịch và báo lệch
- Mức lương một giờ, giữ lịch sử khi thay đổi
- Đánh giá trình độ, nhận xét, khen thưởng
- **Khấu trừ vi phạm:** đi trễ tính tự động từ chấm công; các lỗi khác (đồng phục, dùng điện thoại…) Quản lý/Admin ghi tay — mỗi loại lỗi có ngưỡng miễn phạt và bậc phạt tăng dần riêng. Xem [[QT-04 - Lương]] (BR-34 đến BR-36)
- Xem nhanh tài sản HLV đang giữ — chi tiết ở [[TS - Tài sản và dụng cụ]]

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S6 | Check-in / check-out | HLV | 1 nút. Tự nhận buổi gần giờ hiện tại nhất |

## Quy trình liên quan

- P3 — Buổi dạy tại sân (nguồn chấm công)
- P5 — Chốt lương kỳ (tiêu thụ dữ liệu chấm công và khấu trừ vi phạm)

## Quy tắc nghiệp vụ liên quan

- [[QT-04 - Lương]] — BR-21 đến BR-25, BR-34 đến BR-36 (chấm công và vi phạm là đầu vào của cách tính lương)

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — hồ sơ mình · đăng ký lịch rảnh · check-in/out

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`staff` · `staff_availability` · `staff_pay_rate` · `staff_review` · `timesheet` · `violation_type` · `violation_penalty_tier` · `violation_log`

## Quyết định liên quan

- [[QĐ-04 - HLV tự chấm công bằng check-in, check-out]]
- [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]]
- [[QĐ-10 - Không cần khai báo trình độ HLV để xếp lịch]]
- [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]]
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú / câu hỏi mở

- Không có vai trò Kế toán riêng — xem [[QĐ-06 - Không có vai trò Kế toán riêng]].
