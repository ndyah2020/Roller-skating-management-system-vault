---
ma: TS
ten: "Tài sản và dụng cụ"
loai: module
giai_doan: 3
trang_thai: Đã đặc tả
tags:
  - module
---

# TS - Tài sản và dụng cụ

> Giai đoạn 3 · Sổ giao–nhận giày và dụng cụ, chống thất lạc.

## Mục đích

Theo dõi ai đang giữ tài sản gì của CLB, và luân chuyển tài sản giữa các HLV nhanh, ít thao tác (đúng NT-1, NT-5).

## Ranh giới với BH

| Loại giày | Thuộc về | Module |
|---|---|---|
| Giày thử của CLB | Tài sản | TS |
| Giày đặt dư, giữ lại bán tiếp | Tồn kho, vẫn có người giữ | BH |
| Giày đã bán chưa giao | Đơn hàng | BH |

Quyết định gốc: [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]].

## Chức năng chính

- Danh mục tài sản: giày patin thử · mũ · bộ bảo hộ · cọc slalom · loa · balo
- Gắn mã riêng cho từng chiếc khi cần theo dõi cá thể
- **Sổ giao–nhận:** ai đang giữ gì, từ ngày nào, ai xác nhận — thao tác phải nhanh vì đổi người giữ liên tục
- Chặn quy trình nghỉ việc khi HLV còn giữ đồ
- Cho học viên thuê giày theo buổi → sinh khoản thu bên TC
- Cấp phát không thu hồi cho HLV (balo, cốc, bánh xe) → sinh một khoản chi
- Bảo trì, ghi nhận hư hỏng và mất; kiểm kê định kỳ

**Đã đổi so với bản trước:** sổ giao–nhận của TS (`asset_assignment`) gộp chung với người giữ tồn kho của BH (`stock_item.holder_staff_id`) thành một sổ duy nhất — vì giày thử và hàng tồn đều "giao cho một HLV ngẫu nhiên giữ", tra một chỗ cho nhanh.

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S8 | Chuyển giao tài sản | HLV | 3 chạm: chọn món → chọn người nhận → xác nhận. Người nhận xác nhận lại trên máy họ |
| S9 | Tôi đang giữ gì | HLV | Danh sách phẳng, có nút giao nhanh cho từng dòng |

## Quy trình liên quan

- P6 — Giao nhận giày và dụng cụ

## Quy tắc nghiệp vụ liên quan

- [[QT-05 - Tài sản và hàng hoá]] — BR-26 đến BR-29

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — xem và giao nhận món mình giữ

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`asset_type` · `asset` · `custody_log` (sổ giao–nhận gộp chung với BH) · `staff_supply` · `asset_maintenance` · `equipment_rental` · `stock_count`

## Quyết định liên quan

- [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]]

## Ghi chú / câu hỏi mở

- Rủi ro chính: sổ giao–nhận sai vì thao tác quá nhiều — xem mục Rủi ro ở note Tổng quan.
