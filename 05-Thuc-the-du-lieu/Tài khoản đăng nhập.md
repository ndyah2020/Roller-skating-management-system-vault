---
ten: "Tài khoản đăng nhập"
loai: thực thể
bang_db: user
module_chu: HT
trang_thai: Nháp
tags:
  - thuc-the
---

# Tài khoản đăng nhập

> Bảng: `user` · Module chủ: HT

Một dòng = một tài khoản đăng nhập. Gắn với đúng một `staff` (nếu là HLV) hoặc đúng một `guardian` (nếu là phụ huynh); Admin và Quản lý không gắn hồ sơ nào cả — cả hai là vai trò hệ thống thuần, không phải hồ sơ vận hành như HLV/phụ huynh. Học viên (trẻ nhỏ) **không có tài khoản** — xem [[QT-06 - Dữ liệu và kế toán]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]]): `id` (PK) · `created_at` · `updated_at` · `created_by` · `updated_by` · `is_active`

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `username` | text | — | ✓ | Tên đăng nhập, duy nhất |
| `email` | text | — | – | Email liên hệ/đăng nhập, duy nhất nếu có |
| `phone` | text | — | – | Số điện thoại, duy nhất nếu có — nhiều người Việt đăng nhập bằng SĐT |
| `password_hash` | text | — | ✓ | Mật khẩu đã hash, **không** lưu plaintext |
| `role_id` | uuid | FK → `role.id` | ✓ | Vai trò của tài khoản — admin / manager / coach / guardian |
| `staff_id` | uuid | FK → `staff.id` | – | Gắn hồ sơ HLV nếu vai trò là HLV |
| `guardian_id` | uuid | FK → `guardian.id` | – | Gắn hồ sơ phụ huynh nếu vai trò là Phụ huynh |
| `status` | text | — | ✓ | enum: `active` / `locked` / `disabled` — mặc định `active` |
| `last_login_at` | timestamptz | — | – | Lần đăng nhập gần nhất |

## Ràng buộc & chỉ mục

- Unique: `username`; `email` (khi khác NULL); `phone` (khi khác NULL)
- Check gợi ý: không được vừa có `staff_id` vừa có `guardian_id` cùng lúc (một tài khoản chỉ gắn một loại hồ sơ, hoặc không gắn gì nếu là Admin/Quản lý)
- Index: `role_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-06 - Dữ liệu và kế toán]] — tài khoản tách khỏi hồ sơ học viên/HLV

## Ghi chú

- Mọi cột kết thúc bằng `_by` ở các bảng khác (`created_by`, `marked_by`, `confirmed_by`…) đều trỏ tới `user.id` — xem [[Quy ước đặt tên]].
- ❓ Câu hỏi mở: nếu sau này CLB cần trả lương cố định cho Quản lý như một nhân viên văn phòng (khác với lương theo buổi của HLV), sẽ cần thêm hồ sơ nhân sự riêng cho Quản lý hoặc mở rộng `staff`. Hiện tại chưa có yêu cầu này nên để Quản lý không gắn hồ sơ, giữ đơn giản — xem [[QĐ-09 - Tách vai trò Quản lý khỏi Admin]].
