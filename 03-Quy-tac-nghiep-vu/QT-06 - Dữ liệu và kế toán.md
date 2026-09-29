---
ma: QT-06
ten: "Dữ liệu và kế toán"
loai: quy tắc
ap_dung_cho: "Toàn hệ thống"
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# QT-06 - Dữ liệu và kế toán

> Áp dụng cho: toàn hệ thống — quản lý chủ yếu ở [[HT - Nền tảng và phân quyền]] và [[TC - Tài chính và học phí]]

| Mã | Quy tắc | Áp dụng ở |
|---|---|---|
| BR-30 | Mọi thao tác chạm tiền hoặc tài sản ghi `created_by` / `updated_by` kèm thời điểm | Toàn hệ thống |
| BR-31 | Không xoá cứng, chỉ đánh dấu `is_active = false` | Toàn hệ thống |
| BR-32 | Kỳ kế toán đã chốt thì khoá, không sửa được số liệu quá khứ | `accounting_period.is_locked` |

## Ba quy tắc xuyên suốt (bối cảnh)

1. Mọi thao tác chạm tới tiền hoặc tài sản đều ghi vết. **Không xoá cứng**, chỉ đánh dấu huỷ.
2. Tài khoản đăng nhập tách khỏi hồ sơ học viên và HLV — trẻ nhỏ không có tài khoản, phụ huynh mới là người đăng nhập.
3. Buổi học (`session`) là trục chính: điểm danh, chấm công HLV, trừ buổi trong gói đều móc vào nó.

## Câu hỏi mở

Không có câu hỏi mở ngoài phần thiết kế database.
