---
ten: "Biến động số buổi"
loai: thực thể
bang_db: credit_transaction
module_chu: DH
trang_thai: Nháp
tags:
  - thuc-the
---

# Biến động số buổi

> Bảng: `credit_transaction` · Module chủ: DH

Một dòng = một lần số buổi còn lại của một gói học tăng/giảm. Điểm danh **không trừ thẳng** vào gói — sinh dòng này ở trạng thái chờ, Admin chốt ngày mới thật sự cộng/trừ (xem [[QĐ-02 - Không tự động trừ buổi, chốt cuối ngày]]).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `student_package_id` | uuid | FK → `student_package.id` | ✓ | Gói bị ảnh hưởng |
| `session_id` | uuid | FK → `session.id` | – | Buổi liên quan — để trống nếu là điều chỉnh thủ công không gắn buổi |
| `attendance_id` | uuid | FK → `attendance.id` | – | Dòng điểm danh sinh ra biến động này, nếu có |
| `amount` | numeric(5,2) | — | ✓ | Âm là trừ, dương là hoàn. Cho phép 0.5 (BR-18) |
| `reason` | text | — | – | **Bắt buộc** nếu Admin thao tác thủ công (BR-19) |
| `decided_by` | uuid | FK → `user.id` | – | Ai chốt — null nếu chưa chốt |
| `decided_at` | timestamptz | — | – | Lúc chốt |
| `is_confirmed` | boolean | — | ✓ | Đã chốt hay còn chờ — mặc định `false` |

## Ràng buộc & chỉ mục

- Index: `student_package_id`
- Index: `is_confirmed` (để P4 lọc nhanh danh sách chờ chốt)

## Quy tắc nghiệp vụ áp dụng

- [[QT-03 - Trừ buổi]] — toàn bộ BR-13 đến BR-20 và BR-33
- [[QĐ-07 - Nửa buổi 1-1 chuyển nhóm, CLB bù phần thiếu HLV]] — trường hợp `amount = -0.5`

## Ghi chú

- `is_confirmed` là tầng kiểm tra **từng buổi** (BR-33, được phép để sang ngày sau). Tầng kiểm tra **số học viên** bắt buộc cùng ngày (BR-15) hiện chưa gắn cột riêng — xem câu hỏi mở ở [[QT-03 - Trừ buổi]].
