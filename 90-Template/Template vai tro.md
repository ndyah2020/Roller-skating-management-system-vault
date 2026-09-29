<%*
/* Tên file = tên vai trò  (ví dụ: HLV, Admin, Phụ huynh - Học viên) */
let title = tp.file.title;
if (/^(Untitled|Không có tiêu đề|Chưa đặt tên)/i.test(title)) {
  const input = await tp.system.prompt("Tên vai trò", "");
  if (input && input.trim()) { title = input.trim(); await tp.file.rename(title); }
}
_%>
---
ten: <% JSON.stringify(title) %>
loai: vai trò
tags:
  - vai-tro
---

# <% title %>

> Thiết bị chính: 

Một câu: vai trò này là ai, làm gì trong CLB.

## Việc làm nhiều nhất

- 

## Không được làm

- 

## Quyền theo module

| Module | Quyền |
|---|---|
|  |  |

## Ghi chú

- 
