---
ten: "Danh mục dùng chung"
loai: thực thể
bang_db: category
module_chu: HT
trang_thai: Nháp
tags:
  - thuc-the
---

# Danh mục dùng chung

> Bảng: `category` · Module chủ: HT

Một bảng cho mọi danh mục nhỏ, ít thay đổi, không cần bảng riêng (nguồn khách, phương thức thanh toán, loại chi phí…). Phân biệt danh mục nào với nhau qua `group_code`.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `group_code` | text | — | ✓ | Nhóm danh mục, ví dụ `lead_source`, `expense_type`, `payment_method` |
| `code` | text | — | ✓ | Mã giá trị trong nhóm, ví dụ `facebook` |
| `name` | text | — | ✓ | Tên hiển thị tiếng Việt |
| `sort_order` | integer | — | – | Thứ tự hiển thị, mặc định 0 |

## Ràng buộc & chỉ mục

- Unique: (`group_code`, `code`)
- Index: `group_code`

## Ghi chú

- Không dùng bảng này cho danh mục đã có bảng riêng và có quan hệ nghiệp vụ rõ (`level`, `asset_type`…) — chỉ dùng cho danh mục "phẳng", không cần cột riêng ngoài tên.
