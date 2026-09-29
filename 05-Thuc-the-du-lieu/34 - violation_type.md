---
ten: "Loại lỗi vi phạm"
loai: thực thể
bang_db: violation_type
module_chu: NS
so_thu_tu: 34
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Loại lỗi vi phạm"
---

# violation_type

> #34 · Loại lỗi vi phạm · Module chủ: NS

Một dòng = một loại lỗi có thể bị khấu trừ lương (đi trễ, đồng phục, dùng điện thoại…). Danh mục mở — thêm loại lỗi mới không cần đổi schema.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `code` | text | — | ✓ | Mã ngắn, duy nhất — ví dụ `late`, `uniform`, `phone_use` |
| `name` | text | — | ✓ | Tên hiển thị |
| `basis` | text | — | ✓ | enum: `duration` (tính theo số phút — dùng cho đi trễ) / `count` (tính theo số lần vi phạm — dùng cho các lỗi còn lại) |
| `free_allowance` | integer | — | ✓ | Ngưỡng miễn phạt — số phút nếu `basis = duration`, số lần nếu `basis = count` |
| `count_reset_period` | text | — | – | Chỉ dùng khi `basis = count`: mốc reset số lần đã vi phạm. enum: `payroll_period` / `month` / `never`. Để trống nếu `basis = duration` |
| `note` | text | — | – | Ghi chú |

## Ràng buộc & chỉ mục

- Unique: `code`

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]] — BR-34, BR-35
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú

- ❓ Mặc định `count_reset_period` cho loại lỗi mới: đề xuất `payroll_period` (hết kỳ lương thì số lần vi phạm đếm lại từ 0) — cần bạn xác nhận, xem câu hỏi mở ở [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]].
- Dữ liệu mẫu minh hoạ khi triển khai (không phải giá trị chốt cuối): `late` (`duration`, `free_allowance = 30` phút) · `uniform` (`count`, `free_allowance = 3` lần) · `phone_use` (`count`, tự chọn ngưỡng).
