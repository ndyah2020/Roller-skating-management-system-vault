---
ten: "Phân quyền"
loai: thực thể
bang_db: permission
module_chu: HT
trang_thai: Nháp
tags:
  - thuc-the
---

# Phân quyền

> Bảng: `permission` · Module chủ: HT

Một dòng = một vai trò được phép làm một hành động trên một tài nguyên (màn hình/nhóm dữ liệu). Ma trận quyền theo module ở note Tổng quan hệ thống là bản rút gọn dễ đọc của bảng này.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `role_id` | uuid | FK → `role.id` | ✓ | Vai trò được cấp quyền |
| `resource` | text | — | ✓ | Tên tài nguyên/màn hình, ví dụ `session`, `invoice`, `payroll` |
| `action` | text | — | ✓ | enum: `view` / `create` / `edit` / `delete` / `approve` |

## Ràng buộc & chỉ mục

- Unique: (`role_id`, `resource`, `action`)

## Ghi chú / câu hỏi mở

- [ ] ❓ Mức chi tiết hiện để theo **màn hình/tài nguyên** (`resource`), chưa xuống tới từng bản ghi (ví dụ "chỉ sửa hồ sơ HLV do mình phụ trách"). Nếu sau này cần chi tiết hơn, thêm cột `scope` (ví dụ `own` / `all`) thay vì tách bảng mới.
