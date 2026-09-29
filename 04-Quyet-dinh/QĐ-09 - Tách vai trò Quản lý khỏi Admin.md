---
ma: QĐ-09
ten: "Tách vai trò Quản lý khỏi Admin"
loai: quyết định
ngay: 2026-09-28
trang_thai: Đã chốt
anh_huong:
  - "[[HT - Nền tảng và phân quyền]]"
  - "[[DH - Dạy học]]"
  - "[[NS - Nhân sự]]"
  - "[[HV - Học viên và phụ huynh]]"
  - "[[TC - Tài chính và học phí]]"
  - "[[LG - Lương và thù lao]]"
  - "[[TS - Tài sản và dụng cụ]]"
  - "[[BH - Bán hàng và đặt giày]]"
  - "[[MK - Marketing và tuyển sinh]]"
  - "[[BC - Báo cáo và thông báo]]"
tags:
  - quyet-dinh
---

# QĐ-09 - Tách vai trò Quản lý khỏi Admin

> Ngày: 2026-09-28

## Bối cảnh

CLB phát triển tới quy mô cần một người vận hành hằng ngày (xếp lịch, quản HLV, học viên, bán hàng…) không nhất thiết là người chủ CLB. Nếu chỉ có một vai trò Admin duy nhất, người vận hành hằng ngày sẽ phải được cấp toàn quyền hệ thống — kể cả quyền phân quyền (tạo/xoá tài khoản, đổi quyền người khác) — vượt quá nhu cầu thực tế của công việc đó.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ một vai trò Admin duy nhất | Đơn giản, không thêm vai trò | Người vận hành hằng ngày (không phải chủ CLB) buộc phải có quyền phân quyền — rủi ro bảo mật |
| B. Thêm vai trò Quản lý, tách quyền vận hành hằng ngày khỏi quyền phân quyền hệ thống | Đúng với thực tế phân công (chủ CLB ≠ người vận hành), giới hạn rủi ro ở quyền phân quyền | Thêm một vai trò, thêm điều kiện kiểm tra quyền theo vai trò trong code |

## Quyết định

**Chọn phương án B.** Thêm vai trò [[Quản lý]], đứng giữa [[Admin]] và [[HLV]] · [[Phụ huynh - Học viên]]:

- **[[Quản lý]]** — toàn quyền vận hành hằng ngày trên 9 module: [[DH - Dạy học]] · [[NS - Nhân sự]] · [[HV - Học viên và phụ huynh]] · [[TC - Tài chính và học phí]] · [[LG - Lương và thù lao]] · [[TS - Tài sản và dụng cụ]] · [[BH - Bán hàng và đặt giày]] · [[MK - Marketing và tuyển sinh]] · [[BC - Báo cáo và thông báo]]. **Không** có quyền trên [[HT - Nền tảng và phân quyền]] — không tạo/xoá tài khoản, không đổi vai trò hoặc quyền của người khác, không chỉnh cấu hình hệ thống.
- **[[Admin]]** — giữ toàn quyền trên mọi module, kể cả mọi việc Quản lý làm được. [[HT - Nền tảng và phân quyền]] là module Admin độc quyền, Quản lý không có.

Mô hình là **cấp bậc** (Admin đứng trên, bao trùm Quản lý), không phải hai vai trò tách biệt ngang hàng.

## Lý do

- Người vận hành hằng ngày (thuê ngoài hoặc không phải chủ CLB) không cần và không nên có quyền phân quyền hệ thống — tách quyền đó ra giảm rủi ro nếu tài khoản Quản lý bị lộ hoặc dùng sai.
- Admin (chủ CLB) vẫn cần toàn quyền để giám sát và can thiệp khi cần — quyết định này chỉ thêm một vai trò mới có quyền hẹp hơn, không rút quyền nào của Admin.
- CLB nhỏ chưa có Quản lý riêng vẫn hoạt động bình thường: Admin tự làm hết, vai trò Quản lý chỉ dùng khi CLB thật sự tuyển người vận hành riêng.

## Hệ quả

- Thêm role mới `manager` trong [[Danh mục vai trò]] (bảng `role`).
- [[Tài khoản đăng nhập]] (bảng `user`): tài khoản Quản lý không gắn hồ sơ nào, giống Admin — xem lý do và câu hỏi mở ở note đó.
- Cập nhật bảng Tác nhân và bảng Phân quyền theo module ở [[Tổng quan hệ thống]].
- Cập nhật mục "Vai trò liên quan" ở 9 module (trừ HT) để thêm [[Quản lý]]; module HT ghi rõ Quản lý không có quyền truy cập.
- [[QĐ-06 - Không có vai trò Kế toán riêng]] vẫn còn hiệu lực — xem phần Cập nhật ở note đó.
