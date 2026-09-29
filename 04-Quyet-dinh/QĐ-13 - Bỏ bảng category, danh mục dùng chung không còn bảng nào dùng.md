---
ma: QĐ-13
ten: "Bỏ bảng category, danh mục dùng chung không còn bảng nào dùng"
loai: quyết định
ngay: 2026-09-29
trang_thai: Đã chốt
anh_huong:
  - "[[HT - Nền tảng và phân quyền]]"
tags:
  - quyet-dinh
---

# QĐ-13 - Bỏ bảng category, danh mục dùng chung không còn bảng nào dùng

> Ngày: 2026-09-29

## Bối cảnh

Khung thiết kế database ban đầu có bảng `category` (Danh mục dùng chung) — dự tính làm một bảng chung cho các danh mục nhỏ, ít thay đổi: trình độ, loại tài sản, nguồn khách, phương thức thanh toán. Rà lại toàn bộ 64 bảng so với phần đặc tả nghiệp vụ (theo yêu cầu rà soát bảng dư thừa, đọc lại toàn bộ `01-Module` đến `05-Thuc-the-du-lieu`) phát hiện bảng này không còn được dùng thật ở đâu.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ `category`, dùng cho các danh mục nhỏ còn lại (nguồn khách, phương thức thanh toán…) | Có sẵn một chỗ chung nếu sau này cần | Hiện tại không có cột `uuid` FK nào trỏ vào `category.id` — hai cột từng dự tính dùng (`expense.expense_type`, `lead.source`) đều là `text` với enum cố định, không thật sự nối vào bảng này |
| B. Bỏ hẳn `category` | Đơn giản, không giữ một bảng không ai dùng | Nếu sau này CLB cần thêm danh mục dùng chung thật sự, phải tạo lại bảng |

## Quyết định

**Chọn phương án B.** Bỏ bảng `category` khỏi `05-Thuc-the-du-lieu/`.

## Lý do

Hai mục đích ban đầu của `category` đã có bảng riêng từ trước: trình độ → [[Trình độ]] (`level`), loại tài sản → [[Loại tài sản]] (`asset_type`). Hai mục đích còn lại (nguồn khách ở `lead.source`, phương thức thanh toán ở `payment.method`, loại chi phí ở `expense.expense_type`) đều đã và đang được thiết kế là cột `text` với enum cố định, không phải cột `uuid` FK trỏ vào `category.id`. Kiểm tra toàn vault: không có bảng nào FK thật sự vào `category.id` — giống hệt lý do đã bỏ `staff_teaching_level` ở [[QĐ-10 - Không cần khai báo trình độ HLV để xếp lịch]], bảng nằm trong khung nháp nhưng chưa từng được dùng trong quy tắc nghiệp vụ nào.

Hai bảng đánh dấu *(tuỳ chọn)* trong tài liệu gốc ([[Ghi chú y tế]], [[Đánh giá HLV]]) **không** bị coi là dư thừa cùng đợt này — "tuỳ chọn/ưu tiên thấp" khác với "dư thừa/không liên quan": cả hai vẫn gắn với nhu cầu nghiệp vụ thật (an toàn y tế học viên, đánh giá nhân sự), chỉ là có thể để trống nếu CLB chưa cần dùng ngay.

## Hệ quả

- Bỏ bảng `category` (`06 - category.md`) — xoá khỏi `05-Thuc-the-du-lieu/`. Tổng số bảng: 64 → 63.
- Đánh số lại `so_thu_tu` và đổi tên file cho 58 bảng từ #07 trở đi (dồn xuống #06 → #63) để số thứ tự vẫn liền mạch — không ảnh hưởng wikilink cũ vì Obsidian phân giải theo `aliases`, không theo tên file.
- [[HT - Nền tảng và phân quyền]] — bớt một bảng (7 → 6 bảng); bớt một dòng trong "Chức năng chính".
- `expense.expense_type` và `lead.source` — sửa lại mô tả cột, bỏ tham chiếu chết tới `category`, giữ nguyên là enum cố định (không đổi kiểu dữ liệu, không đổi ý nghĩa).
- Nếu sau này CLB thật sự cần một danh mục nhỏ dùng chung mới (không đáng tạo bảng riêng), tạo lại `category` lúc đó cũng không tốn kém — không mất gì khi bỏ bây giờ.
