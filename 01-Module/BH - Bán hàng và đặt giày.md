---
ma: BH
ten: "Bán hàng và đặt giày"
loai: module
giai_doan: 3
trang_thai: Đã đặc tả
tags:
  - module
---

# BH - Bán hàng và đặt giày

> Giai đoạn 3 · Chỉ đặt nhà cung cấp khi đã có người mua, không ôm tồn kho chủ động.

## Mục đích

Nhận đơn đặt giày theo yêu cầu của khách, gom đơn thành đợt đặt, và xử lý hàng đặt dư thành tồn kho bán tiếp.

## Luồng đơn

Khách chọn giày → Đặt hàng → Hàng về → Giao khách

Nhánh rẽ: huỷ đơn · đổi size · khách không nhận · **đặt dư** → trả lại nhà cung cấp, hoặc giữ làm tồn kho bán tiếp.

## Chức năng chính

- Danh mục sản phẩm: giày theo hãng/model/size · bảo hộ · đồng phục · bánh và bạc đạn
- Giá nhập, giá bán, biên lợi nhuận
- Đo và lưu size chân học viên
- Nhận đơn kèm cọc, in phiếu hẹn
- Gom nhiều đơn thành một đợt đặt khi đủ số lượng hoặc đủ ngưỡng giá trị
- Theo dõi ngày về, nhắc khách, ghi nhận bàn giao
- **Hàng đặt dư:** chuyển thành tồn kho, giao cho một người giữ, bán tiếp sau
- Bảo hành và đổi size

## Màn hình

| Mã | Màn hình | Tác nhân | Mục tiêu thao tác |
|---|---|---|---|
| S10 | Đơn đặt giày | Admin | Lọc theo trạng thái, biết đơn nào đang nằm ở HLV nào |

## Quy trình liên quan

- P7 — Đặt giày cho khách

## Quy tắc nghiệp vụ liên quan

- [[QT-05 - Tài sản và hàng hoá]] — BR-28, BR-29

## Vai trò liên quan

- [[Admin]] — toàn quyền
- [[Quản lý]] — toàn quyền
- [[HLV]] — xem đơn giao tại sân mình
- [[Phụ huynh - Học viên]] — xem đơn của mình

## Dữ liệu liên quan

*(chỉ tên bảng — schema chi tiết làm ở `05-Thuc-the-du-lieu`)*

`product` · `supplier` · `customer_order` · `purchase_batch` · `goods_receipt` · `stock_item` (giữ chung sổ giao–nhận với [[TS - Tài sản và dụng cụ]]) · `warranty_return`

## Quyết định liên quan

- [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]]

## Ghi chú / câu hỏi mở

Không có câu hỏi mở ngoài phần thiết kế database.
