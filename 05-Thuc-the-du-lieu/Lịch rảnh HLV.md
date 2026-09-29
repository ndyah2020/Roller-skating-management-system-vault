---
ten: "Lịch rảnh HLV"
loai: thực thể
bang_db: staff_availability
module_chu: NS
trang_thai: Nháp
tags:
  - thuc-the
---

# Lịch rảnh HLV

> Bảng: `staff_availability` · Module chủ: NS

HLV tự đăng ký khung giờ rảnh; Admin đối chiếu bảng này khi xếp lịch tuần (P2).

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `staff_id` | uuid | FK → `staff.id` | ✓ | HLV |
| `weekday` | smallint | — | ✓ | Thứ trong tuần |
| `start_time` | time | — | ✓ | Giờ bắt đầu rảnh |
| `end_time` | time | — | ✓ | Giờ kết thúc rảnh |
| `effective_from` | date | — | ✓ | Áp dụng từ ngày |
| `effective_to` | date | — | – | Áp dụng tới ngày, để trống nếu còn hiệu lực |

## Ràng buộc & chỉ mục

- Check: `end_time` > `start_time`
- Index: (`staff_id`, `weekday`)
