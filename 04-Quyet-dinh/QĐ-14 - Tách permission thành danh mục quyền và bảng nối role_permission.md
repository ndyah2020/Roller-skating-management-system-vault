---
ma: QĐ-14
ten: "Tách permission thành danh mục quyền và bảng nối role_permission"
loai: quyết định
ngay: 2026-09-29
trang_thai: Đã chốt
anh_huong:
  - "[[HT - Nền tảng và phân quyền]]"
tags:
  - quyet-dinh
---

# QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission

> Ngày: 2026-09-29

## Bối cảnh

Thiết kế ban đầu gộp cột `role_id` thẳng vào bảng `permission` — mỗi dòng vừa là một quyền (`resource` + `action`) vừa gắn cứng với một vai trò cụ thể, coi quan hệ role–permission như 1-nhiều thay vì đúng bản chất nhiều-nhiều (một quyền có thể cấp cho nhiều vai trò, một vai trò có nhiều quyền). Khi được hỏi tại sao không tách bảng nối đúng chuẩn, lý do ban đầu là hệ thống chỉ có 4 vai trò cố định (Admin, Quản lý, HLV, Phụ huynh) nên gộp thẳng không sai lệch dữ liệu, chỉ chưa chuẩn hoá.

Xác nhận với người dùng: **muốn sau này có thể thêm vai trò mới linh hoạt, không cố định ở 4 vai trò ban đầu.**

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ nguyên `role_id` trong `permission` | Ít bảng hơn, đủ dùng khi chỉ có 4 vai trò cố định | Thêm vai trò mới phải thêm cả loạt dòng `permission` trùng lặp (mỗi quyền × vai trò mới); `permission` phình to theo số vai trò thay vì theo số quyền |
| B. Tách 3 bảng chuẩn RBAC: `role`, `permission` (bỏ `role_id`), `role_permission` (bảng nối) | Thêm vai trò mới chỉ cần thêm dữ liệu, không đổi schema; `permission` là danh mục thuần, không trùng lặp | Thêm 1 bảng, truy vấn quyền của một vai trò phải join qua bảng nối |

## Quyết định

**Chọn phương án B.** Tách `permission` thành 2 phần: `permission` (danh mục quyền thuần, bỏ cột `role_id`) và `role_permission` (bảng nối nhiều-nhiều mới, #03) — một dòng = một vai trò được cấp một quyền cụ thể.

## Lý do

Thêm vai trò mới sau này (ví dụ "Kế toán", "Lễ tân") chỉ cần thêm 1 dòng vào `role` và các dòng tương ứng vào `role_permission` — không phải đổi schema, không phải sửa cấu trúc bảng `permission`. `permission` trở thành danh mục thuần, số dòng chỉ tăng khi có tài nguyên/hành động mới chứ không tăng theo số vai trò — đúng chuẩn hoá quan hệ nhiều-nhiều, tránh trùng lặp dữ liệu.

## Hệ quả

- Thêm bảng mới `role_permission` (#03) — chèn ngay sau `role` (#01) và `permission` (#02). Các bảng từ `user` (cũ #03) trở đi đánh số lại lên 1 (#04 → #64). Tổng số bảng: 63 → 64.
- `permission.md` — bỏ cột `role_id` và ràng buộc unique liên quan; unique mới chỉ còn (`resource`, `action`).
- `role.md` — sửa lại mô tả: không còn "chỉ 4 dòng cố định" mà là "bảng mở, khởi tạo 4 dòng, thêm được không giới hạn".
- [[HT - Nền tảng và phân quyền]] — thêm `role_permission` vào danh sách bảng liên quan.
- Vai trò thêm sau này chưa có tài liệu nghiệp vụ riêng ở `02-Vai-tro/` — cấu hình quyền hoàn toàn qua dữ liệu ở `role` + `role_permission`, không bắt buộc phải viết note nghiệp vụ mới (vẫn có thể viết thêm nếu muốn mô tả rõ vai trò đó làm gì).
- Không ảnh hưởng các module khác ngoài HT.
