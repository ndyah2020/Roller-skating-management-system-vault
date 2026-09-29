---
ma: QT-05
ten: "Tài sản và hàng hoá"
loai: quy tắc
ap_dung_cho: "TS, BH"
trang_thai: Hiệu lực
tags:
  - quy-tac
---

# QT-05 - Tài sản và hàng hoá

> Áp dụng cho: [[TS - Tài sản và dụng cụ]] · [[BH - Bán hàng và đặt giày]]

| Mã | Quy tắc | Áp dụng ở |
|---|---|---|
| BR-26 | Giày thử và hàng tồn kho dùng **chung một sổ giao–nhận**. Người giữ không cố định, HLV nào cũng có thể giữ | `custody_log` |
| BR-27 | HLV **không được duyệt nghỉ việc** khi còn giữ món chưa trả | P5, quy trình nghỉ việc |
| BR-28 | Hàng đặt dư **mặc định chuyển thành tồn kho** và giao cho một HLV giữ. Trả lại nhà cung cấp là ngoại lệ hiếm | `stock_item` |
| BR-29 | Giày khách đặt được giao **tại sân**, nên luôn phải biết đơn nào đang nằm ở ai | `customer_order` + `custody_log` |

Ranh giới TS–BH (giày thử = tài sản, giày đặt dư = tồn kho): xem [[QĐ-03 - Ranh giới tài sản và hàng tồn kho giữa TS và BH]].

## Câu hỏi mở

Không có câu hỏi mở ngoài phần thiết kế database.
