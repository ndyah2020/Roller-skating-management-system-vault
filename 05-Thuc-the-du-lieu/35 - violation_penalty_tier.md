---
ten: "Mức phạt theo bậc"
loai: thực thể
bang_db: violation_penalty_tier
module_chu: NS
so_thu_tu: 35
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Mức phạt theo bậc"
---

# violation_penalty_tier

> #35 · Mức phạt theo bậc · Module chủ: NS

Một dòng = một bậc phạt của một [[Loại lỗi vi phạm]] — mức phạt tăng dần theo bậc, áp dụng chung cho mọi HLV.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `violation_type_id` | uuid | FK → `violation_type.id` | ✓ | Loại lỗi mà bậc này thuộc về |
| `threshold_from` | integer | — | ✓ | Mốc bắt đầu áp dụng bậc này (số phút trễ nếu `basis = duration`, số lần vi phạm thứ mấy nếu `basis = count`) |
| `penalty_amount` | bigint | — | ✓ | Số tiền phạt ở bậc này, VND |

## Ràng buộc & chỉ mục

- Unique: (`violation_type_id`, `threshold_from`)

## Quy tắc nghiệp vụ áp dụng

- [[QT-04 - Lương]] — BR-34, BR-35
- [[QĐ-12 - Khấu trừ vi phạm theo bậc, dùng một khung chung cho mọi loại lỗi]]

## Ghi chú

- Cách đọc: lấy bậc có `threshold_from` lớn nhất mà vẫn ≤ giá trị vi phạm đo được (`violation_log.measured_value`) của lần đó.
- Ví dụ minh hoạ cho `late` (`free_allowance = 30`): bậc `threshold_from = 30` → `penalty_amount = 20.000` · bậc `threshold_from = 35` → `penalty_amount = 30.000`. Số phút/số tiền ở đây theo đúng lời mô tả ban đầu nhưng chưa chốt.
- Ví dụ minh hoạ cho `uniform` (`free_allowance = 3`): bậc `threshold_from = 4` → `penalty_amount = 20.000` · bậc `threshold_from = 5` → `penalty_amount = 40.000` — cũng chỉ là ví dụ.
- Một loại lỗi chưa có dòng nào ở đây = loại lỗi đó khai báo rồi nhưng chưa áp dụng phạt tiền.
