<%*
/* Tên file dạng "MÃ - Tên module"  (ví dụ: DH - Dạy học)
   Quy ước đầy đủ: note [[Quy ước đặt tên]] */
let title = tp.file.title;
if (!/^[A-Z]+ - /.test(title)) {
  const input = await tp.system.prompt("Tên note dạng  MÃ - Tên module   (ví dụ: DH - Dạy học)", title);
  if (input && input.trim() && input.trim() !== title) { title = input.trim(); await tp.file.rename(title); }
}
const sep = title.indexOf(" - ");
const ma  = sep > 0 ? title.slice(0, sep).trim() : title.trim();
const ten = sep > 0 ? title.slice(sep + 3).trim() : "";
const yten = JSON.stringify(ten);
_%>
---
ma: <% ma %>
ten: <% yten %>
loai: module
giai_doan: 
trang_thai: Chưa làm
tags:
  - module
---

# <% title %>

> Giai đoạn: · Trạng thái: Chưa làm

%% Giá trị trang_thai · giai_doan: xem [[Quy ước đặt tên]]. %%

## Mục đích

2–3 câu: module này giải quyết việc gì, sai ở đây thì hỏng gì.

## Chức năng chính

- 

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
|  |  |  |  |

## Quy trình liên quan

- 

## Quy tắc nghiệp vụ liên quan

- [[QT-xx - ]]

## Vai trò liên quan

- [[]] — 

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`` · ``

## Quyết định liên quan

- [[QĐ-xx - ]]

## Ghi chú / câu hỏi mở

- [ ] ❓ 
