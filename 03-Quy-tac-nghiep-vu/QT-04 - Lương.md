---
ma: QT-04
ten: "Lương"
loai: quy tắc
ap_dung_cho: "NS, LG"
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# QT-04 - Lương

> Áp dụng cho: [[NS - Nhân sự]] · [[LG - Lương và thù lao]]

| Mã    | Quy tắc                                                                                                                                                                | Áp dụng ở                             |
| ----- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------- |
| BR-21 | Chấm công bằng HLV **tự check-in / check-out** tại sân.                                                                                                               | `timesheet.variance_minutes`          |
| BR-22 | Các buổi trong ngày của một HLV gom thành **cụm**. Hai buổi liên tiếp cùng cụm nếu khoảng cách ≤ **2 giờ** (cấu hình được — xem [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]]) | P5                                    |
| BR-23 | Công của một cụm tính từ **giờ bắt đầu buổi đầu tới giờ kết thúc buổi cuối**, kể cả giờ trống bên trong — dùng khi cụm được chọn là **ca thường** (BR-24, BR-25)      | P5                                    |
| BR-24 | Mỗi cụm được **chọn thủ công** là **ca thường** hoặc **ca lẻ** — không tự động suy ra từ số buổi trong cụm. Mặc định **ca thường**. Chọn nhầm thì tự sửa lại hoặc Admin sửa, miễn còn trước khi chốt kỳ lương | `payroll_line.line_type`              |
| BR-25 | **Ca thường:** lương = đơn giá theo giờ ([[Mức lương theo giờ]]) × giờ công của cụm (BR-23). **Ca lẻ:** lương = mức cố định, mặc định 100.000đ, cấu hình được ([[Quy tắc tính công]]) — **thay thế hoàn toàn** cách tính theo giờ, không cộng thêm | `pay_rule` · `payroll_line.amount`    |
| BR-34 | **Đi trễ** tính **tự động** từ chấm công (`timesheet.variance_minutes`, phần trễ). Trong ngưỡng miễn phạt của loại lỗi ([[Loại lỗi vi phạm]], ví dụ minh hoạ 30 phút) thì không phạt; vượt ngưỡng thì phạt theo bậc tăng dần theo số phút trễ, cấu hình ở [[Mức phạt theo bậc]] | `violation_log` (`source = system`)   |
| BR-35 | Các lỗi **khác** (đồng phục, dùng điện thoại, và các loại lỗi bổ sung sau này) do Quản lý hoặc Admin **ghi tay** vào [[Nhật ký vi phạm]]. Mỗi loại lỗi có ngưỡng số lần được miễn phạt riêng; vượt ngưỡng thì phạt theo bậc tăng dần theo số lần vi phạm, cấu hình ở [[Mức phạt theo bậc]] | `violation_log` (`source = manual`)   |
| BR-36 | Mỗi lần vi phạm có phạt tiền sinh một dòng khấu trừ trong lương (`payroll_line.line_type = deduction`, nối qua cột đa hình `reference_type = 'violation_log'`). Còn sửa/xoá được, miễn kỳ lương chưa chốt | `payroll_line`                        |

## Ví dụ kiểm chứng BR-22 đến BR-25

| Cụm trong ngày                    | Giờ công của cụm | Chọn **ca thường** | Chọn **ca lẻ**            |
| ---------------------------------- | ---------------- | ------------------- | -------------------------- |
| 9–10h (một buổi rồi về)            | 1 giờ             | đơn giá × 1          | 100.000đ (không tính giờ)  |
| 17–21h (nhiều buổi liên tiếp)      | 4 giờ             | đơn giá × 4          | 100.000đ (không tính giờ)  |
| 9h→12h (2 buổi cách nhau ≤ 2 giờ)  | 3 giờ             | đơn giá × 3          | 100.000đ (không tính giờ)  |

Ví dụ lương cả ngày: cụm 9–10h chọn **ca lẻ**, cụm 17–21h chọn **ca thường** → lương ngày = 100.000 + đơn giá × 4.

## Ví dụ kiểm chứng BR-34 đến BR-36 (minh hoạ — số liệu chưa chốt)

| Loại lỗi (`violation_type.code`) | `basis`    | Miễn phạt (`free_allowance`) | Bậc phạt ví dụ ([[Mức phạt theo bậc]])      |
| --------------------------------- | ---------- | ----------------------------- | -------------------------------------------- |
| `late` — đi trễ                   | `duration` | 30 phút                       | từ phút 30: 20.000đ · từ phút 35: 30.000đ    |
| `uniform` — đồng phục              | `count`    | 3 lần                         | lần thứ 4: 20.000đ · lần thứ 5: 40.000đ      |
| `phone_use` — dùng điện thoại      | `count`    | tự chọn                       | tự chọn                                       |

Ví dụ đọc bậc: HLV trễ 36 phút → vượt 30 phút (miễn phạt) → so hai bậc 30 và 35, lấy bậc lớn nhất ≤ 36 → bậc `threshold_from = 35` → phạt 30.000đ (không cộng dồn với bậc 30). Số phút và số tiền trong ví dụ này đúng theo lời mô tả ban đầu, chưa phải số chốt cuối — xem câu hỏi mở ở [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]].

## Vì sao đổi từ "phụ cấp cộng thêm" sang "lương thay thế, chọn thủ công"

Bản đầu ([[QĐ-01 - Phụ cấp ca lẻ là số tiền cố định, cấu hình được]]) coi ca lẻ là một khoản **cộng thêm** vào lương giờ, và hệ thống **tự suy ra** ca lẻ khi cụm chỉ có một buổi một giờ. Nay đổi theo [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]]: ca lẻ là một cách tính lương **khác** cho cụm đó (100.000đ thẳng, không cộng dồn với giờ công), và người xử lý lương **tự chọn** ca thường/ca lẻ cho từng cụm — không còn quy tắc cứng "đúng 1 buổi 1 giờ mới tính ca lẻ".

## Khấu trừ vi phạm dùng một khung chung (BR-34 đến BR-36)

Đi trễ và các lỗi khác (đồng phục, điện thoại, và lỗi mở rộng sau này) dùng **chung một cơ chế** phạt-theo-bậc, chỉ khác ở cách đo (`basis`):

- **`duration`** (hiện chỉ có đi trễ): giá trị đo là **số phút** vượt ngưỡng miễn phạt, hệ thống tự tính từ `timesheet.variance_minutes` — không cần Quản lý/Admin ghi tay.
- **`count`** (đồng phục, điện thoại, và các lỗi bổ sung sau): giá trị đo là **số lần vi phạm thứ mấy** trong kỳ hiện tại, Quản lý hoặc Admin ghi tay từng lần vào [[Nhật ký vi phạm]].

Cả hai đều tra bậc phạt theo cùng một cách ở [[Mức phạt theo bậc]], và phạt tiền (nếu có) đều chảy vào lương qua [[Dòng cấu thành lương]] theo BR-36. Thêm một loại lỗi mới chỉ cần thêm một dòng ở [[Loại lỗi vi phạm]] và các bậc phạt tương ứng — không cần đổi schema hay code. Xem [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]].

## Câu hỏi mở

- Ca lẻ (BR-24, BR-25): không có câu hỏi mở — đã chốt ở [[QĐ-11 - Ca lẻ là lương thay thế, chọn thủ công thay vì tự động suy ra]]. Ngưỡng gộp cụm (BR-22): đã chốt ở [[QĐ-05 - Ngưỡng gộp ca lẻ là 2 giờ]].
- Khấu trừ vi phạm (BR-34 đến BR-36): 2 câu hỏi mở, xem [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]] — mặc định `count_reset_period`, và toàn bộ số phút/số tiền ví dụ ở trên chưa phải số chốt cuối.
