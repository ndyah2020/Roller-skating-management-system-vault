---
ten: "Khung giờ của sân"
loai: thực thể
bang_db: venue_time_slot
module_chu: HT
trang_thai: Nháp
tags:
  - thuc-the
---

# Khung giờ của sân

> Bảng: `venue_time_slot` · Module chủ: HT

Một dòng = một khung giờ mà một sân có thể mở lớp, lặp lại theo thứ trong tuần.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `venue_id` | uuid | FK → `venue.id` | ✓ | Sân áp dụng |
| `weekday` | smallint | — | ✓ | 1–7 (Thứ 2 – Chủ nhật, quy ước cụ thể chốt khi code) |
| `start_time` | time | — | ✓ | Giờ bắt đầu khung |
| `end_time` | time | — | ✓ | Giờ kết thúc khung |
| `note` | text | — | – | Ghi chú tự do |

## Ràng buộc & chỉ mục

- Check: `end_time` > `start_time`
- Index: `venue_id`

## Quy tắc nghiệp vụ áp dụng

- [[QT-01 - Lớp và lịch]] — dùng để đối chiếu khi xếp lịch tuần (P2)
