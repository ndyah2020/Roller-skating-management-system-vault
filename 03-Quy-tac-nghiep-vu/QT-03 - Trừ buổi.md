---
ma: QT-03
ten: "Trừ buổi"
loai: quy tắc
ap_dung_cho: "DH, TC"
trang_thai: Hiệu lực
tags:
  - quy-tac
---
# QT-03 - Trừ buổi

> Áp dụng cho: [[DH - Dạy học]] · [[TC - Tài chính và học phí]]

| Mã    | Quy tắc                                                                                              | Áp dụng ở                   |
| ----- | ----------------------------------------------------------------------------------------------------- | ---------------------------- |
| BR-13 | Khoá mặc định **10 buổi**, cấu hình được nhiều hơn                                                   | `package.session_count`     |
| BR-14 | **Chỉ trừ khi học viên thực học.** Không ra sân thì không trừ gì cả                                  | `credit_transaction`        |
| BR-15 | Quản lý **bắt buộc** xác nhận **số học viên** đã học trong ngày trước khi hết ngày. Chưa xác nhận → cảnh báo `attendance_not_closed` tiếp tục nhắc, không tự tắt | P4 · `alert` |
| BR-16 | Buổi huỷ **do mưa** → không trừ buổi của bất kỳ học viên nào                                         | `session.cancel_reason`     |
| BR-17 | Trừ theo **loại hình thực tế dạy**, không theo loại đã đăng ký                                       | `session.actual_course_id`  |
| BR-18 | Nửa buổi (0.5) chỉ dùng cho buổi hỗn hợp: học viên đăng ký 1-1 nhưng thiếu HLV nên nửa buổi học nhóm | `credit_transaction.amount` |
| BR-19 | Admin trừ hoặc hoàn thủ công bất cứ lúc nào **trước khi chốt kỳ kế toán**, bắt buộc ghi lý do        | `credit_transaction.reason` |
| BR-20 | Học viên học được nửa buổi hoặc về sớm có thể chú thích học bù thêm giờ cho buổi sau                 | `credit_transaction.note`   |
| BR-33 | Kiểm tra chi tiết **từng buổi** của từng học viên (từng dòng chờ chốt) không bắt buộc xong trong ngày — được để sang ngày sau, ngồi soát và xác nhận lại lúc đó cũng được | P4 · `credit_transaction` |


Vì vậy điểm danh **không trừ thẳng** vào gói. Nó sinh ra một dòng biến động ở trạng thái chờ mục này sẽ vẫn còn đó không mất đi; Quản lý sửa hoặc huỷ được trước khi chốt ngày (P4). Chốt xong mới ghi vào số buổi còn lại của gói. Xem [[QĐ-02 - Không tự động trừ buổi, chốt cuối ngày]] và [[QĐ-07 - Nửa buổi 1-1 chuyển nhóm, CLB bù phần thiếu HLV]] (BR-18).

## Hai tầng kiểm tra khi chốt ngày (BR-15, BR-33)

"Chốt ngày" (P4) không phải một việc duy nhất, mà có hai tầng khác mức bắt buộc:

1. **Số học viên đã học hôm nay — bắt buộc, cùng ngày (BR-15).** Quản lý phải xác nhận tổng số học viên đã học hôm nay là đúng — coi như bước soát nhanh, không được bỏ qua. Chưa xác nhận thì cảnh báo `attendance_not_closed` (xem [[Cảnh báo cần xử lý]]) vẫn còn treo, hệ thống tiếp tục nhắc.
2. **Từng buổi của từng học viên — được phép trễ (BR-33).** Xem, sửa, hoặc xác nhận từng dòng biến động (xem [[Biến động số buổi]]) có thể để sang ngày hôm sau — dữ liệu chờ chốt vẫn còn đó, không mất, ngồi soát lại lúc nào cũng được.

Tách hai tầng để Quản lý không bị kẹt lại quá lâu cuối mỗi ngày, nhưng vẫn có một bước chặn nhanh chống quên hẳn nhiều ngày (đúng rủi ro đã ghi ở [[Tổng quan hệ thống]], mục Rủi ro).

## Vì sao cần `session.cancel_reason`

Ba việc thật, không chỉ để ghi chú:

1. **Quyết định có trừ buổi hay không.** Huỷ do mưa thì không trừ của ai cả (BR-16). Huỷ do học viên báo nghỉ thì Admin cân nhắc.
2. **Tính công HLV.** HLV đã tới sân mà buổi bị huỷ vì mưa là một chuyện; buổi bị huỷ từ hôm trước là chuyện khác.
3. **Báo cáo.** Sân nào hay huỷ vì mưa thì biết mà tính chuyện mái che hoặc đổi khung giờ.

Cột này **bắt buộc khi huỷ buổi**, chọn từ danh sách: mưa · HLV bận · sân bận · học viên báo nghỉ · lý do khác kèm ghi chú.

## Câu hỏi mở

- ❓ BR-15 (số học viên) hiện chưa gắn vào một cột dữ liệu cụ thể — có thể chỉ cần dựa vào cảnh báo `attendance_not_closed` được đánh `is_resolved = true` (xem [[Cảnh báo cần xử lý]]), không cần thêm cột riêng. Để ngỏ, thuộc phần thiết kế database.
