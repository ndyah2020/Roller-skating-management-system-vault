---
ten: "Danh mục vai trò"
loai: thực thể
bang_db: role
module_chu: HT
so_thu_tu: 1
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Danh mục vai trò"
---

# role

> #01 · Danh mục vai trò · Module chủ: HT

Danh mục vai trò đăng nhập — khởi tạo 4 vai trò (Admin, Quản lý, HLV, Phụ huynh) nhưng **bảng mở**, thêm vai trò mới được mà không cần đổi schema (xem [[QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission]]). Khác với các note mô tả vai trò ở `02-Vai-tro` (đó là tài liệu nghiệp vụ cho 4 vai trò khởi tạo; bảng này là dữ liệu để `user.role_id` trỏ vào, có thể có thêm dòng ngoài 4 vai trò đó).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột           | Kiểu | Khoá | Bắt buộc | Mô tả                                                           |
| ------------- | ---- | ---- | -------- | --------------------------------------------------------------- |
| `code`        | text | —    | ✓        | `admin` / `manager` / `coach` / `guardian` — mã dùng trong code |
| `name`        | text | —    | ✓        | Tên hiển thị: Admin, Quản lý, HLV, Phụ huynh                    |
| `description` | text | —    | –        | Ghi chú thêm                                                    |


## Ràng buộc & chỉ mục

- Unique: `code`
- Dữ liệu khởi tạo: 4 dòng (`admin`, `manager`, `coach`, `guardian`) — bảng **mở**, thêm vai trò mới được, không giới hạn 4

## Ghi chú

- Xem mô tả nghiệp vụ đầy đủ ở [[Admin]], [[Quản lý]], [[HLV]], [[Phụ huynh - Học viên]] — 4 vai trò khởi tạo. Vai trò thêm sau này cấu hình quyền qua [[Phân quyền]] + [[Gán quyền cho vai trò]], chưa cần note nghiệp vụ riêng.
- Thêm `manager` — xem [[QĐ-09 - Tách vai trò Quản lý khỏi Admin]].
- Tách `role_id` ra khỏi `permission` thành bảng nối riêng ([[Gán quyền cho vai trò]]) để thêm vai trò mới không phải đổi schema — xem [[QĐ-14 - Tách permission thành danh mục quyền và bảng nối role_permission]].
