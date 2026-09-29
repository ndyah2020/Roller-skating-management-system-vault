---
ten: "Phân quyền"
loai: thực thể
bang_db: permission
module_chu: HT
so_thu_tu: 2
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Phân quyền"
---

# permission

> #02 · Phân quyền · Module chủ: HT

Danh mục các quyền có thể cấp — một dòng = một hành động trên một tài nguyên (màn hình/nhóm dữ liệu), **không gắn vai trò nào cả**. Vai trò nào được quyền nào nằm ở [[Gán quyền cho vai trò]] (`role_permission`) — xem [[QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission]]. Ma trận quyền theo module ở note Tổng quan hệ thống là bản rút gọn dễ đọc của cặp 2 bảng này.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột        | Kiểu | Khoá | Bắt buộc | Mô tả                                                          |
| ---------- | ---- | ---- | -------- | -------------------------------------------------------------- |
| `resource` | text | —    | ✓        | Tên tài nguyên/màn hình, ví dụ `session`, `invoice`, `payroll` |
| `action`   | text | —    | ✓        | enum: `view` / `create` / `edit` / `delete` / `approve`        |

## Ràng buộc & chỉ mục

- Unique: (`resource`, `action`)

## Ghi chú / câu hỏi mở

- [ ] ❓ Mức chi tiết hiện để theo **màn hình/tài nguyên** (`resource`), chưa xuống tới từng bản ghi (ví dụ "chỉ sửa hồ sơ HLV do mình phụ trách"). Nếu sau này cần chi tiết hơn, thêm cột `scope` (ví dụ `own` / `all`) thay vì tách bảng mới.
- Đã tách `role_id` ra khỏi bảng này (bản cũ gộp thẳng vào đây) sang [[Gán quyền cho vai trò]] — để thêm vai trò mới không phải đổi schema. Xem [[QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission]].
