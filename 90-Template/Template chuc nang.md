<%*
/* TUỲ CHỌN — chỉ dùng khi một MÀN HÌNH cụ thể (S1, S2…) quá phức tạp để gói trong
   bảng "Màn hình" của note module. Mặc định KHÔNG cần note riêng: cứ thêm một dòng
   vào bảng "Màn hình" trong note module là đủ. */
let title = tp.file.title;
_%>
---
ten: <% JSON.stringify(title) %>
loai: chức năng
module: 
trang_thai: Chưa làm
tags:
  - chuc-nang
---

# <% title %>

> Module: · Mã màn hình liên quan: 

## Mục đích

2–3 câu.

## Luồng chính

1. 

## Luồng phụ & ngoại lệ

- **Không có quyền** → 
- **Mất mạng** → 
- **Dữ liệu rỗng** → 

## Tiêu chí nghiệm thu

Mỗi dòng phải đo được: có số, có ngưỡng, hoặc có kết quả kiểm tra rõ ràng.

- [ ] 

## Câu hỏi mở

- [ ] ❓ 
