---
ma: QĐ-12
ten: "Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi"
loai: quyết định
ngay: 2026-09-29
trang_thai: Đã chốt
anh_huong:
  - "[[NS - Nhân sự]]"
  - "[[LG - Lương và thù lao]]"
tags:
  - quyet-dinh
---

# QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi

> Ngày: 2026-09-29

## Bối cảnh

CLB cần khấu trừ lương HLV khi vi phạm: đi trễ, đồng phục, dùng điện thoại, và các loại lỗi khác sẽ bổ sung dần sau này. Hai nhóm lỗi có cách phát hiện khác nhau — đi trễ đo được tự động từ chấm công, các lỗi còn lại phải có người ghi nhận — nhưng cách phạt tiền muốn dùng **chung một cơ chế**: mỗi loại lỗi có một ngưỡng miễn phạt riêng, vượt ngưỡng thì phạt tăng dần theo bậc.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Mỗi loại lỗi một bảng/logic riêng (ví dụ bảng riêng cho đi trễ, bảng riêng cho đồng phục…) | Từng bảng đơn giản, dễ đọc riêng lẻ | Thêm loại lỗi mới phải thêm bảng và code mới — không đúng ý muốn "cách thức phạt tương tự nhau" cho mọi lỗi |
| B. Một khung chung: danh mục loại lỗi mở ([[Loại lỗi vi phạm]]) + bậc phạt cấu hình theo từng loại ([[Mức phạt theo bậc]]) + nhật ký vi phạm dùng chung ([[Nhật ký vi phạm]]), phân biệt bằng `basis` (đo theo thời gian hay theo số lần) | Thêm loại lỗi mới chỉ cần thêm dữ liệu (loại lỗi + bậc phạt), không đổi schema hay code; đúng ý muốn dùng chung một cách thức phạt | Một bảng dùng chung cho nhiều loại lỗi khác nhau về ý nghĩa — phải đọc `basis` mới hiểu cột `measured_value`/`threshold_from` đang tính gì lúc đó |

## Quyết định

**Chọn phương án B.**

- 3 bảng mới, module NS: [[Loại lỗi vi phạm]] (`violation_type`), [[Mức phạt theo bậc]] (`violation_penalty_tier`), [[Nhật ký vi phạm]] (`violation_log`).
- `violation_type.basis`: **`duration`** — đi trễ, đo bằng số phút, hệ thống tự tính từ `timesheet.variance_minutes`, không cần ai ghi tay. **`count`** — các lỗi còn lại (đồng phục, điện thoại, và lỗi bổ sung sau này), đo bằng số lần vi phạm, Quản lý hoặc Admin ghi tay.
- Mỗi loại lỗi có `free_allowance` riêng (ngưỡng miễn phạt); vượt ngưỡng thì tra bậc phạt tăng dần ở `violation_penalty_tier`.
- Phạt tiền (nếu có) chảy vào lương qua `payroll_line` bằng cột đa hình `reference_type`/`reference_id` **đã có sẵn** — không thêm FK mới, không thêm cột `payroll_line_id` ở `violation_log`.
- Chi tiết quy tắc: [[QT-04 - Lương]], BR-34 (đi trễ, tự động), BR-35 (lỗi khác, ghi tay), BR-36 (chảy vào lương).

## Lý do

Bạn muốn "cách thức phạt sẽ tương tự như vậy" cho mọi loại lỗi hiện tại và những loại lỗi "sẽ có nhiều lỗi đưa ra" sau này — một khung chung, danh mục mở, tránh phải sửa schema mỗi khi CLB nghĩ ra lỗi mới cần phạt. Tách riêng `basis` (`duration`/`count`) để một khung vẫn xử lý đúng cả hai cách đo rất khác nhau (thời gian trễ vs. số lần vi phạm) mà không cần hai bảng riêng.

## Hệ quả

- [[QT-04 - Lương]] — thêm BR-34, BR-35, BR-36 và ví dụ minh hoạ.
- [[NS - Nhân sự]] — thêm 3 bảng vào "Dữ liệu liên quan"; tách rõ phần khấu trừ vi phạm khỏi mục đánh giá/khen thưởng chung.
- [[LG - Lương và thù lao]] — mục "Khấu trừ" nói rõ dùng khung [[Nhật ký vi phạm]] thay vì chỉ liệt kê chung.
- [[Sơ đồ dữ liệu]] — 61 → 64 bảng; thêm một dòng ví dụ cho `payroll_line` ở bảng cột đa hình.

## Câu hỏi mở

- ❓ Mặc định `count_reset_period` khi tạo loại lỗi mới (áp dụng cho `basis = count`): đề xuất **`payroll_period`** (hết kỳ lương thì số lần vi phạm đếm lại từ 0). Cần bạn xác nhận đúng ý, hay muốn `never` (cộng dồn suốt thời gian làm việc, không bao giờ reset) hay `month`.
- ❓ Toàn bộ số phút (30, 35) và số tiền (20.000đ, 30.000đ, 40.000đ…) dùng làm ví dụ ở [[Mức phạt theo bậc]] và [[QT-04 - Lương]] lấy đúng theo lời bạn mô tả ban đầu ("hay 20 hay 30 đó", "kiểu vậy") — là minh hoạ, **chưa phải số chốt cuối**. Số thật sẽ do bạn nhập làm dữ liệu cấu hình khi triển khai, không nằm cứng trong quy tắc.
