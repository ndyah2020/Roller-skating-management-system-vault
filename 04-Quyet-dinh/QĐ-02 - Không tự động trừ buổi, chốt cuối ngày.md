---
ma: QĐ-02
ten: "Không tự động trừ buổi, chốt cuối ngày"
loai: quyết định
ngay: 2026-09-09
trang_thai: Đã chốt
anh_huong:
  - "[[DH - Dạy học]]"
  - "[[TC - Tài chính và học phí]]"
tags:
  - quyet-dinh
---

# QĐ-02 - Không tự động trừ buổi, chốt cuối ngày

> Ngày: 2026-09-09

## Bối cảnh

Nếu trừ buổi ngay khi điểm danh, một sự cố (mất mạng, tích nhầm, quên tích) sẽ trừ sai mà không ai kịp sửa trước khi nó ảnh hưởng tới số buổi còn lại của học viên.

## Các phương án đã cân nhắc

| Phương án                                                | Ưu                                            | Nhược                           |
| -------------------------------------------------------- | --------------------------------------------- | ------------------------------- |
| A. Trừ ngay khi điểm danh                                | Đơn giản, không cần bước chốt                 | Sai là mất, khó sửa kịp         |
| B. Sinh biến động chờ, Admin chốt cuối ngày mới trừ thật | An toàn, sửa được trước khi ảnh hưởng dữ liệu | Thêm một bước thao tác mỗi ngày |
|                                                          |                                               |                                 |

## Quyết định

**Chọn phương án B.** Không tự động trừ mù. Mỗi lượt điểm danh sinh một dòng biến động ở trạng thái chờ; Admin xem, sửa hoặc bỏ qua; bấm chốt mới thật sự trừ vào gói. Không thao tác gì thì cuối ngày hệ thống không bỏ qua và vẫn ở đó, bắt buộc phải xác nhận cuối ngày— **không trừ buổi nào**.

## Lý do

Khớp nguyên tắc NT-2 (mặc định an toàn): không thao tác gì thì không mất tiền, không mất buổi.

## Hệ quả

- [[DH - Dạy học]] — quy trình P4 (chốt ngày)
- [[TC - Tài chính và học phí]] — số buổi còn lại của gói chỉ đổi sau khi chốt
- Xem thêm: [[QT-03 - Trừ buổi]]
