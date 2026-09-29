<%*
/* Tên file dạng "QT-xx - Tên nhóm quy tắc"  (ví dụ: QT-01 - Lớp và lịch)
   Một note QT-xx gom NHIỀU quy tắc BR-xx cùng chủ đề — không phải 1 note = 1 quy tắc. */
let title = tp.file.title;
if (!/^QT-\d+ - /.test(title)) {
  const input = await tp.system.prompt("Tên note dạng  QT-xx - Tên nhóm quy tắc   (ví dụ: QT-01 - Lớp và lịch)", title);
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
loai: quy tắc
ap_dung_cho: 
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# <% title %>

> Áp dụng cho: 

Mỗi dòng giữ mã `BR-xx` riêng để tham chiếu khi viết code và viết test — không đổi số khi thêm/sửa.

| Mã | Quy tắc | Áp dụng ở |
|---|---|---|
|  |  |  |

## Ghi chú thêm

- 

## Câu hỏi mở

- [ ] ❓ 
