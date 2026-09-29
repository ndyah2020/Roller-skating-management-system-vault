---
ma: QT-01
ten: "Lớp và lịch"
loai: quy tắc
ap_dung_cho: "DH"
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# QT-01 - Lớp và lịch

> Áp dụng cho: [[DH - Dạy học]]

| Mã    | Quy tắc                                                                                                                                                                                                                                                           | Áp dụng ở                                |
| ----- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------- |
| BR-01 | Một HLV kèm tối đa **4 học viên** có thể chỉnh sửa được sau này với lớp nhóm; lớp 1-1 là một HLV một học viên                                                                                                                                                     | `course.max_student_per_coach`           |
| BR-02 | **Lúc xếp lịch không gán học viên cho HLV nào.** Chỉ cần biết buổi đó có đủ HLV hay không. Quan hệ ai dạy bé nào **suy ra sau buổi học** từ người tích điểm danh (BR-08), không khai báo trước. Nguyện vọng chọn HLV chỉ là gợi ý mềm, thay người khác vẫn hợp lệ | `session_coach` · `attendance.marked_by` |
| BR-03 | Sức chứa một khung giờ tại một sân = tổng sức chứa của các HLV được phân vào khung giờ đó                                                                                                                                                                         | P2, cảnh báo xếp lịch                    |
| BR-04 | Một học viên **không được đăng ký hai sân khác nhau trong cùng một ngày**                                                                                                                                                                                         | `enrollment`, ràng buộc khi ghi danh     |
| BR-05 | Lịch sửa tự do miễn là **trước giờ buổi học bắt đầu**; sau đó chỉ được huỷ kèm lý do                                                                                                                                                                              | `session`                                |
| BR-06 | Một HLV không được phân hai buổi trùng giờ, kể cả khác sân                                                                                                                                                                                                        | `session_coach`                          |

## Ghi chú thêm

- BR-03 là lý do `venue` không cần cột sức chứa cố định — sức chứa suy ra động từ số HLV được phân.
- BR-02 là lý do hệ thống **không** có bảng ghép nhóm học viên-HLV: `session_coach` chỉ ghi buổi đó có những HLV nào, hết.

## Câu hỏi mở

Không có câu hỏi mở ngoài phần thiết kế database.
