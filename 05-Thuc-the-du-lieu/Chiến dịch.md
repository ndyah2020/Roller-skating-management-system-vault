---
ten: "Chiến dịch"
loai: thực thể
bang_db: campaign
module_chu: MK
trang_thai: Nháp
tags:
  - thuc-the
---

# Chiến dịch

> Bảng: `campaign` · Module chủ: MK

+ 6 cột chung mọi bảng (xem [[Quy ước đặt tên]])

## Cột riêng của bảng

| Cột | Kiểu | Khoá | Bắt buộc | Mô tả |
|---|---|---|---|---|
| `name` | text | — | ✓ | Tên chiến dịch |
| `channel` | text | — | – | Kênh chạy (Facebook, TikTok…) |
| `start_date` | date | — | – | Ngày bắt đầu |
| `end_date` | date | — | – | Ngày kết thúc |
| `cost` | bigint | — | ✓ | Chi phí — mặc định 0 |
| `lead_count` | integer | — | ✓ | Số khách quan tâm thu được — mặc định 0 |
| `conversion_count` | integer | — | ✓ | Số học viên chuyển đổi thành công — mặc định 0 |
