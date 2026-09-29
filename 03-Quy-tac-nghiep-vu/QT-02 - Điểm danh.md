---
ma: QT-02
ten: "Điểm danh"
loai: quy tắc
ap_dung_cho: "DH"
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# QT-02 - Điểm danh

> Áp dụng cho: [[DH - Dạy học]]

| Mã    | Quy tắc                                                                                                                                                         | Áp dụng ở                                             |
| ----- | --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| BR-07 | Danh sách điểm danh hiện ra theo **sân + khung giờ + ngày hôm nay**. Sân lấy từ vị trí HLV check-in, hoặc từ buổi HLV được phân công — thoả một trong hai là đủ | `session` · `timesheet.check_in_at` · `session_coach` |
| BR-08 | **HLV nào tích cho bé nào thì được ghi là người dạy bé đó** trong buổi đó, tại sân đó, giờ đó. Đây là nguồn duy nhất trả lời câu hỏi "ai dạy bé này"            | `attendance.marked_by` · `attendance.marked_at`       |
| BR-09 | Bất kỳ HLV nào thoả BR-07 đều được tích cho bất kỳ bé nào trong danh sách. Không giới hạn theo nhóm                                                             | `attendance`                                          |
| BR-10 | Mỗi học viên chỉ có **đúng một dòng điểm danh cho một buổi**. Ràng buộc đặt ở tầng CSDL, không trông cậy vào giao diện                                          | `UNIQUE (session_id, student_id)`                     |
| BR-11 | Hai HLV tích cùng một bé gần như cùng lúc: **người ghi trước sẽ được tính**. Người sau nhận thông báo "HLV X đã tích lúc HH:mm", không phải thông báo lỗi       | Ghi kiểu upsert, không khoá bi quan                   |
| BR-12 | Tích nhầm người thì HLV **nhận lại được** — đổi `marked_by` sang mình, ghi vào nhật ký thao tác. Sau khi Admin chốt ngày thì chỉ Admin sửa được                 | `attendance` · `audit_log`                            |

Bổ sung: nếu học viên ra bất ngờ (không có trong danh sách), điểm danh vẫn thêm được học viên đó vào danh sách, hoặc ghi note lại để Admin chốt cuối buổi vì thao tác thêm học viên giữa buổi có thể mất thời gian, cần cách lọc/tìm nhanh theo tên tại sân.

## Bốn lớp chống tích trùng

Xếp từ chắc nhất xuống — xem sơ đồ ở note Tổng quan hệ thống:

| Lớp                  | Cách làm                                                                                           | Chặn được gì                                                            |
| -------------------- | -------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------- |
| 1. Ràng buộc CSDL    | `UNIQUE (session_id, student_id)` trên `attendance`                                                | Mọi trường hợp, kể cả hai request vào đúng cùng mili giây               |
| 2. Ghi kiểu upsert   | Cú bấm là "tạo nếu chưa có", không phải "tạo mới". Người sau nhận về dòng đã có kèm tên người tích | Biến lỗi trùng thành thông tin, HLV không thấy màn hình đỏ              |
| 3. Đồng bộ danh sách | Màn hình tự cập nhật vài giây một lần: bé đã tích thì làm mờ và hiện tên người tích                | Thu hẹp khoảng thời gian hai người cùng bấm, từ thường xuyên xuống hiếm |
| 4. Mã lần gửi        | Mỗi cú bấm kèm một mã do máy sinh; gửi lại cùng mã thì server trả về kết quả cũ                    | Mạng ở sân chập chờn, HLV bấm hai ba lần vì tưởng chưa ăn               |

**Không dùng khoá bi quan.** Khoá dòng khi HLV mở danh sách nghe có vẻ an toàn, nhưng điện thoại ở sân hay mất sóng giữa chừng, khoá sẽ kẹt lại và HLV khác không tích được. Ràng buộc duy nhất cộng upsert cho kết quả đúng như nhau mà không có rủi ro đó.

## Câu hỏi mở

Không có câu hỏi mở ngoài phần thiết kế database.
