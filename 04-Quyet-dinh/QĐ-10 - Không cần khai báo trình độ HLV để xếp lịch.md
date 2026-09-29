---
ma: QĐ-10
ten: "Không cần khai báo trình độ HLV để xếp lịch"
loai: quyết định
ngay: 2026-09-28
trang_thai: Đã chốt
anh_huong:
  - "[[NS - Nhân sự]]"
  - "[[DH - Dạy học]]"
tags:
  - quyet-dinh
---

# QĐ-10 - Không cần khai báo trình độ HLV để xếp lịch

> Ngày: 2026-09-28

## Bối cảnh

Khung thiết kế database ban đầu có bảng `staff_teaching_level` (HLV nào được phép dạy trình độ nào), dự tính dùng để chặn xếp lịch sai ở P2 — không xếp một HLV vào lớp có trình độ họ chưa được phép dạy.

## Các phương án đã cân nhắc

| Phương án | Ưu | Nhược |
|---|---|---|
| A. Giữ `staff_teaching_level`, chặn xếp lịch theo trình độ HLV được phép dạy | Đúng lý thuyết, tránh xếp nhầm HLV chưa đủ trình độ | Thêm bảng, thêm việc khai báo và duy trì dữ liệu cho CLB nhỏ — không khớp cách vận hành thực tế |
| B. Bỏ hẳn, xếp lịch chỉ dựa vào số lượng học viên | Đơn giản, khớp thực tế — HLV nào cũng dạy được, chỉ cần đủ số lượng theo tỉ lệ 1:4 | Không có lớp chặn nếu sau này CLB phân cấp HLV theo trình độ |

## Quyết định

**Chọn phương án B.** Bỏ khái niệm "HLV được phép dạy trình độ nào" khỏi việc xếp lịch. Xếp lịch (P2) chỉ cần xác định **hôm đó, sân đó, giờ đó có bao nhiêu học viên**, rồi phân đủ số lượng HLV theo tỉ lệ 1:4 (BR-01, BR-03) — không xét HLV đó được phép dạy trình độ gì.

## Lý do

Đúng với cách [[QT-01 - Lớp và lịch]] đã mô tả từ đầu (BR-02: "lúc xếp lịch không gán học viên cho HLV nào... chỉ cần biết buổi đó có đủ HLV hay không") — bảng `staff_teaching_level` là dư, chưa từng được dùng trong quy tắc nghiệp vụ nào, chỉ nằm trong khung nháp database.

## Hệ quả

- Bỏ bảng `staff_teaching_level` — xoá note "Trình độ được phép dạy" khỏi `05-Thuc-the-du-lieu/`. Tổng số bảng: 62 → 61.
- [[NS - Nhân sự]] — bớt một bảng, bớt việc khai báo/duy trì trình độ cho từng HLV.
- [[DH - Dạy học]] — không đổi gì, P2 vẫn xếp lịch như [[QT-01 - Lớp và lịch]] đã mô tả.
- [[Trình độ]] (bảng `level`) **không đổi** — vẫn giữ để theo dõi trình độ/bài học của học viên, chỉ bỏ phần gắn với HLV.
