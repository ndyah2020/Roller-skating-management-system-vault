---
ten: "Công nợ"
loai: thực thể
bang_db: receivable
module_chu: TC
so_thu_tu: 26
trang_thai: Nháp
tags:
  - thuc-the
aliases:
  - "Công nợ"
---

# receivable

> #26 · Công nợ · Module chủ: TC

Một dòng = phần còn thiếu của một hoá đơn, dùng để nhắc đóng học phí — nhắc **phụ huynh** (chủ thể của hoá đơn ở [[Hoá đơn]]), không theo từng người học riêng vì 1 hoá đơn có thể gồm nhiều người học.

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `invoice_id` | uuid | FK → `invoice.id` | ✓ | Hoá đơn còn nợ |
| `due_amount` | bigint | — | ✓ | Số tiền còn thiếu, VND |
| `due_date` | date | — | ✓ | Hạn đóng |
| `reminder_count` | integer | — | ✓ | Số lần đã nhắc — mặc định 0 |
| `last_reminded_at` | timestamptz | — | – | Lần nhắc gần nhất |
| `status` | text | — | ✓ | enum: `open` / `reminded` / `settled` / `written_off` |

## Ràng buộc & chỉ mục

- Index: `invoice_id`, `due_date`

## Quy tắc nghiệp vụ áp dụng

- [[BC - Báo cáo và thông báo]] — nguồn cho nhắc đóng tiền

## Ghi chú

- Đã bỏ `student_id` (bản cũ trùng thông tin với `invoice`, và hết rõ nghĩa khi 1 hoá đơn gồm nhiều người học) — cần biết phụ huynh nào thì join qua `invoice.guardian_id`. Xem [[QĐ-15 - Hoá đơn theo phụ huynh, cho phép phụ huynh tự học]].
