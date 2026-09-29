<%*
/* Tên file dạng "QĐ-xx - Tiêu đề"  (ví dụ: QĐ-09 - Tên quyết định)
   Quy ước đầy đủ: note [[Quy ước đặt tên]] */
let title = tp.file.title;
if (!/^QĐ-\d+ - /.test(title)) {
  const input = await tp.system.prompt("Tên note dạng  QĐ-xx - Tiêu đề", title);
  if (input && input.trim() && input.trim() !== title) { title = input.trim(); await tp.file.rename(title); }
}
const sep  = title.indexOf(" - ");
const ma   = sep > 0 ? title.slice(0, sep).trim() : title.trim();
const ten  = sep > 0 ? title.slice(sep + 3).trim() : "";
const yten = JSON.stringify(ten);
const ngay = tp.date.now("YYYY-MM-DD");
_%>
---
ma: <% ma %>
ten: <% yten %>
loai: quyết định
ngay: <% ngay %>
trang_thai: Đã chốt
anh_huong: 
tags:
  - quyet-dinh
---

# <% title %>

> Ngày: <% ngay %>

## Bối cảnh

Vấn đề gì cần quyết định. Vì sao phải quyết bây giờ.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. |  |  |
| B. |  |  |

## Quyết định

**Chọn phương án …** — một câu rõ ràng, không "tuỳ trường hợp".

## Lý do

## Hệ quả

Module bị ảnh hưởng ghi vào `anh_huong` (frontmatter, dạng `[[Tên module]]`).

- [[]] — 
