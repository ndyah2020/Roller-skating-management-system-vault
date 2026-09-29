<%*
/* Tên file = tên thực thể tiếng Việt (ví dụ: Buổi học). Tên bảng DB nhập khi tạo.
   Dùng ở giai đoạn thiết kế database — xem 05-Thuc-the-du-lieu. */
let title = tp.file.title;
if (/^(Untitled|Không có tiêu đề|Chưa đặt tên)/i.test(title)) {
  const input = await tp.system.prompt("Tên thực thể (tiếng Việt, ví dụ: Buổi học)", "");
  if (input && input.trim()) { title = input.trim(); await tp.file.rename(title); }
}
const bang = ((await tp.system.prompt("Tên bảng DB — tiếng Anh, snake_case, số nhiều (ví dụ: sessions)", "")) ?? "").trim();
const moduleChu = (await tp.system.suggester(
  ["HT","DH","NS","HV","TC","LG","TS","BH","MK","BC"],
  ["HT","DH","NS","HV","TC","LG","TS","BH","MK","BC"], false, "Module chủ")) ?? "";
_%>
---
ten: <% JSON.stringify(title) %>
loai: thực thể
bang_db: <% bang %>
module_chu: <% moduleChu %>
trang_thai: Nháp
tags:
  - thuc-the
---

# <% title %>

> Bảng: `<% bang || "chưa đặt" %>` · Module chủ: <% moduleChu %>

Một câu: thực thể này là gì, một dòng dữ liệu tương ứng với cái gì ngoài đời.

## Trường dữ liệu

Tên trường tiếng Anh `snake_case`. Ba dòng đầu bắt buộc trên mọi bảng vận hành.

| Trường | Kiểu | Bắt buộc | Mặc định | Ghi chú |
|---|---|---|---|---|
| `id` | uuid | ✓ |  | Khoá chính |
| `created_at` | timestamptz | ✓ | now() |  |
| `updated_at` | timestamptz | ✓ | now() |  |
| `` |  |  |  |  |

### Giá trị enum

| Trường enum | Bộ giá trị | Ghi chú |
|---|---|---|
| `` |  |  |

## Quan hệ

| Trường khoá ngoại | Trỏ tới | Quan hệ | Ghi chú |
|---|---|---|---|
| `` |  |  |  |

## Ràng buộc & chỉ mục

- Unique: 
- Index: 
- Check: 

## Ghi chú

- 

## Câu hỏi mở

- [ ] ❓ 
