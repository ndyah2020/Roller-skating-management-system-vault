---
ten: "Nhật ký thao tác"
loai: thực thể
bang_db: audit_log
module_chu: HT
so_thu_tu: 7
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Nhật ký thao tác"
---

# audit_log

> #07 · Nhật ký thao tác · Module chủ: HT

Một dòng = một lần một tài khoản thay đổi dữ liệu tiền hoặc tài sản. **Log bất biến** — chỉ thêm, không sửa, không xoá.

**Khác cột chung:** chỉ có `id` (PK) và `created_at` — **không có** `updated_at` / `updated_by` / `is_active` vì bảng này không sửa sau khi ghi. Người thao tác đã có ở cột `user_id` riêng nên không cần thêm `created_by`.

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `user_id` | uuid | FK → `user.id` | ✓ | Ai thao tác |
| `table_name` | text | — | ✓ | Bảng bị tác động, ví dụ `credit_transaction` |
| `record_id` | uuid | — | ✓ | Id của dòng bị tác động (không FK cứng — có thể trỏ tới bất kỳ bảng nào) |
| `action` | text | — | ✓ | enum: `create` / `update` / `delete` / `restore` |
| `old_value` | jsonb | — | – | Giá trị trước khi đổi |
| `new_value` | jsonb | — | – | Giá trị sau khi đổi |

## Ràng buộc & chỉ mục

- Index: (`table_name`, `record_id`) — để tra nhanh lịch sử một dòng cụ thể
- Index: `user_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-06 - Dữ liệu và kế toán]] — BR-30 (bắt buộc ghi vết mọi thao tác chạm tiền/tài sản)
