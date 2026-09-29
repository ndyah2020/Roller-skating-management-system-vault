---
ten: "Kỳ lương"
loai: thực thể
bang_db: payroll_period
module_chu: LG
so_thu_tu: 37
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Kỳ lương"
---

# payroll_period

> #37 · Kỳ lương · Module chủ: LG

Một dòng = một kỳ trả lương (tuần hoặc tháng), chứa nhiều [[Bảng công theo kỳ]] — một dòng mỗi HLV.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `period_type` | text | — | ✓ | enum: `weekly` / `monthly` |
| `start_date` | date | — | ✓ | Ngày bắt đầu kỳ |
| `end_date` | date | — | ✓ | Ngày kết thúc kỳ |
| `status` | text | — | ✓ | enum: `open` / `closed` |
| `closed_at` | timestamptz | — | – | Lúc chốt kỳ |

## Ràng buộc & chỉ mục

- Check: `end_date` > `start_date`
