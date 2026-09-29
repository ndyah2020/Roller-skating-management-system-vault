---
ten: "Gán quyền cho vai trò"
loai: thực thể
bang_db: role_permission
module_chu: HT
so_thu_tu: 3
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Gán quyền cho vai trò"
---

# role_permission

> #03 · Gán quyền cho vai trò · Module chủ: HT

Bảng nối nhiều-nhiều thật sự giữa [[Danh mục vai trò]] (`role`) và [[Phân quyền]] (`permission`) — một dòng = một vai trò được cấp một quyền cụ thể. Tách riêng khỏi `permission` để thêm vai trò mới (ngoài 4 vai trò khởi tạo) chỉ cần thêm dữ liệu, không phải đổi schema. Xem [[QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission]].

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `role_id` | uuid | FK → `role.id` | ✓ | Vai trò được cấp quyền |
| `permission_id` | uuid | FK → `permission.id` | ✓ | Quyền được cấp |

## Ràng buộc & chỉ mục

- Unique: (`role_id`, `permission_id`)
- Index: `permission_id`

## Ghi chú

- Vai trò thêm sau 4 vai trò khởi tạo (Admin, Quản lý, HLV, Phụ huynh) chưa có tài liệu nghiệp vụ riêng ở `02-Vai-tro/` — cấu hình quyền hoàn toàn qua dữ liệu ở đây, không cần sửa note nghiệp vụ.
