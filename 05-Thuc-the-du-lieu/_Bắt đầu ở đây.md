---
loai: ghi chú
tags:
  - thuc-the
---

# Thực thể dữ liệu — mục lục

Khung nháp 64 bảng đã viết xong theo đúng phần đặc tả nghiệp vụ ở `01-Module` đến `04-Quyet-dinh`. Đây là bản để bạn **xem xét và chỉnh sửa dần** — không phải bản chốt cuối.

- **Quy ước kiểu dữ liệu, cột chung, cột đa hình, và những chỗ tôi đã tự quyết:** xem [[Sơ đồ dữ liệu]].
- **Danh sách đủ 64 bảng theo module:** cũng ở [[Sơ đồ dữ liệu]] (có bảng Dataview tự liệt kê).
- **Từng bảng một note riêng**, dùng `Template thuc the.md` — mỗi note có đủ cột, kiểu, khoá chính/khoá ngoại, ràng buộc, và quy tắc nghiệp vụ áp dụng.

## Việc gợi ý làm tiếp khi bạn chỉnh sửa

1. Đọc [[Sơ đồ dữ liệu]] trước — đặc biệt mục "cột đa hình" (những cột không có FK cứng ở tầng CSDL) và mục "đã tự quyết", vì đó là chỗ dễ cần sửa theo ý bạn nhất.
2. Sửa từng note thực thể trực tiếp — đổi kiểu, thêm/bớt cột, đổi enum. Cột chung (`id`, `created_at`…) chỉ sửa một chỗ ở [[Quy ước đặt tên]], không phải sửa lại 62 note.
3. Khi một bảng đã chốt hẳn, đổi `trang_thai: Nháp` → `trang_thai: Đã chốt` trong frontmatter của note đó.
4. Các câu hỏi mở còn lại: xem cuối [[Sơ đồ dữ liệu]] và mục "Câu hỏi mở" trong từng note liên quan.

## Tài liệu tham khảo

- "Bản đồ module (bản cũ, tham khảo khi thiết kế DB).md" — bản schema chi tiết nhất trước đây, nhưng đã cũ so với khung hiện tại (còn `skill` riêng, còn `makeup_session`, còn `venue.capacity`…) — chỉ để đối chiếu, không copy thẳng.
