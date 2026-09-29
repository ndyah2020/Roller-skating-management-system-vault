---
ten: "Danh mục vai trò"
loai: thực thể
bang_db: role
module_chu: HT
trang_thai: Nháp
tags:
  - thuc-the
---

# Danh mục vai trò

> Bảng: `role` · Module chủ: HT

Danh mục 4 vai trò đăng nhập: Admin, Quản lý, HLV, Phụ huynh. Khác với các note mô tả vai trò ở `02-Vai-tro` (đó là tài liệu nghiệp vụ; bảng này là dữ liệu để `user.role_id` trỏ vào).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `code` | text | — | ✓ | `admin` / `manager` / `coach` / `guardian` — mã dùng trong code |
| `name` | text | — | ✓ | Tên hiển thị: Admin, Quản lý, HLV, Phụ huynh |
| `description` | text | — | – | Ghi chú thêm |

## Ràng buộc & chỉ mục

- Unique: `code`
- Dữ liệu khởi tạo: chỉ 4 dòng cố định (admin, manager, coach, guardian) — hiếm khi thêm dòng mới

## Ghi chú

- Xem mô tả nghiệp vụ đầy đủ ở [[Admin]], [[Quản lý]], [[HLV]], [[Phụ huynh - Học viên]].
- Thêm `manager` — xem [[QĐ-09 - Tách vai trò Quản lý khỏi Admin]].
