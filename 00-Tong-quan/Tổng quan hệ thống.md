---
loai: tổng quan
ngay_cap_nhat: 2026-09-28
tags:
  - tong-quan
---

## Phạm vi

- **Mô hình:** 1 CLB, nhiều sân.
- **Vai trò:** [[Admin]] · [[Quản lý]] · [[HLV]] · [[Phụ huynh - Học viên]].
- **10 module:** [[HT - Nền tảng và phân quyền|HT]] · [[DH - Dạy học|DH]] · [[NS - Nhân sự|NS]] · [[HV - Học viên và phụ huynh|HV]] · [[TC - Tài chính và học phí|TC]] · [[LG - Lương và thù lao|LG]] · [[TS - Tài sản và dụng cụ|TS]] · [[BH - Bán hàng và đặt giày|BH]] · [[MK - Marketing và tuyển sinh|MK]] · [[BC - Báo cáo và thông báo|BC]].
- Quy ước đặt tên, mã, trạng thái dùng chung: xem [[Quy ước đặt tên]].

## 1. Mục tiêu & nguyên tắc

| #    | Nguyên tắc                       | Ý nghĩa khi thiết kế                                                                                                                                                             |     |
| ---- | --------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --- |
| NT-1 | **Ít thao tác nhất**             | Mọi việc làm hằng ngày phải đo được bằng số chạm. Việc làm càng nhiều lần thì càng phải ít bước.                                                                                 |     |
| NT-2 | Mặc định an toàn                 | Không thao tác gì thì hệ thống **không trừ tiền, không trừ buổi với các trường hợp hi hữu, thời tiết**. Chỉ trừ khi có người xác nhận hoặc tự trừ cho học viên đã được tích chọn |     |
| NT-3 | Không xoá cứng                   | Dữ liệu tiền và tài sản chỉ đánh dấu huỷ, luôn biết ai làm và lúc nào.                                                                                                           |     |
| NT-4 | Lịch là thứ thay đổi liên tục    | Cho sửa tự do trước giờ học; ràng buộc chỉ chặn cái thật sự sai.                                                                                                                 |     |
| NT-5 | Làm được trên điện thoại tại sân | HLV không mở máy tính. Điểm danh, check-in, giao giày phải chạy trên điện thoại.                                                                                                 |     |

## 2. Tác nhân

| Tác nhân                 | Thiết bị chính     | Việc làm nhiều nhất                                 | Không được làm                             |
| ------------------------ | ------------------ | --------------------------------------------------- | ------------------------------------------ |
| **Admin** (chủ CLB)      | Máy tính           | Cấu hình phân quyền, giám sát, duyệt việc lớn       | —                                          |
| **Quản lý** (vận hành)   | Máy tính           | Xếp lịch tuần, chốt ngày, chốt lương, duyệt thu chi | Phân quyền, tạo/xoá tài khoản              |
| **HLV**                  | Điện thoại tại sân | Check-in/out, điểm danh, nhận và giao giày          | Sửa gói học, xem lương người khác, sửa giá |
| **Phụ huynh / Học viên** | Điện thoại         | Xem lịch, số buổi còn lại, bài đang học             | Mọi thao tác ghi dữ liệu vận hành          |

Chi tiết từng vai trò: [[Admin]] · [[Quản lý]] · [[HLV]] · [[Phụ huynh - Học viên]]. Ranh giới quyền giữa Admin và Quản lý: xem [[QĐ-09 - Tách vai trò Quản lý khỏi Admin]].

### Phân quyền theo module

| Module | Admin      | Quản lý        | HLV                                                                 | Phụ huynh                      |
| ------ | ---------- | -------------- | ------------------------------------------------------------------- | ------------------------------ |
| HT     | Toàn quyền | Không có quyền | Xem hồ sơ mình                                                      | Xem hồ sơ mình                 |
| DH     | Toàn quyền | Toàn quyền     | Xem lịch mình · điểm danh buổi ở sân mình đang có mặt · ghi tiến độ | Xem lịch và tiến độ của con    |
| NS     | Toàn quyền | Toàn quyền     | Hồ sơ mình · đăng ký lịch rảnh · check-in/out                       | Không                          |
| HV     | Toàn quyền | Toàn quyền     | Xem toàn bộ học viên của buổi mình được phân                        | Hồ sơ con mình                 |
| TC     | Toàn quyền | Toàn quyền     | Không                                                               | Xem hoá đơn và số buổi còn lại |
| LG     | Toàn quyền | Toàn quyền     | Xem phiếu lương của mình                                            | Không                          |
| TS     | Toàn quyền | Toàn quyền     | Xem và giao nhận món mình giữ                                       | Không                          |
| BH     | Toàn quyền | Toàn quyền     | Xem đơn giao tại sân mình                                           | Xem đơn của mình               |
| MK     | Toàn quyền | Toàn quyền     | Không                                                               | Không                          |
| BC     | Toàn quyền | Toàn quyền     | Cảnh báo liên quan tới mình                                         | Thông báo gửi cho mình         |

## 3. Quy trình nghiệp vụ cốt lõi

| Mã  | Quy trình                 | Ai chạy   | Tần suất                  | Module chính |
| --- | ------------------------- | --------- | ------------------------- | ------------ |
| P1  | Ghi danh và bán gói       | Admin     | Khi có học viên mới       | HV · TC      |
| P2  | Xếp lịch tuần             | Admin     | Hằng tuần, sửa trong tuần | DH           |
| P3  | Buổi dạy tại sân          | HLV       | Mỗi buổi                  | DH · NS      |
| P4  | Chốt ngày                 | Admin     | Cuối mỗi ngày             | DH · TC      |
| P5  | Chốt lương kỳ             | Admin     | Tuần hoặc tháng           | LG · NS      |
| P6  | Giao nhận giày và dụng cụ | HLV ↔ HLV | Rất thường xuyên          | TS           |
| P7  | Đặt giày cho khách        | Admin     | Theo đơn                  | BH           |
|     |                           |           |                           |              |

> Theo [[QĐ-09 - Tách vai trò Quản lý khỏi Admin]]: "Admin" ở bảng trên đọc là "Admin hoặc Quản lý" trong thực tế vận hành — Admin chỉ bắt buộc phải tự làm khi CLB chưa có Quản lý riêng.

### P2 — Xếp lịch tuần

```
Đăng ký của học viên (enrollment: sân + thứ + giờ)
        ↓ gom theo (sân, thứ, khung giờ)
Số học viên tại mỗi ô lịch
        ↓ chia theo loại hình đã đăng ký
Số HLV cần   =  số học viên 1-1
              + trần(số học viên nhóm ÷ 4)
        ↓ đối chiếu lịch rảnh HLV
Phân HLV vào buổi  (chỉ cần ĐỦ SỐ LƯỢNG)
        ↓
Cảnh báo nếu thiếu HLV, hoặc HLV trùng giờ ở hai sân
```

Hai điểm cần nhớ — chi tiết ở [[QT-01 - Lớp và lịch]]:

1. Sức chứa một sân không cố định, sức chứa sẽ là tổng của các HLV được phân vào khung giờ đó.
2. Tỉ lệ 1:4 chỉ để đếm số HLV cần, **không** dùng để ghép nhóm trước. HLV nào kèm bé nào là chuyện tự sắp xếp tại chỗ; hệ thống ghi lại sau, qua việc ai tích điểm danh cho bé đó.

### P3 — Điểm danh tại sân

```
HLV check-in tại sân   ─┐
                        ├─ thoả một trong hai là được tích
HLV được phân buổi này ─┘
        ↓
Lọc: sân đó + khung giờ đó + hôm nay
        ↓
Danh sách bé của buổi, mỗi dòng hiện sẵn ai đã tích
        ↓  HLV nào dạy bé nào thì tích bé đó
attendance — mỗi bé chỉ một dòng cho một buổi
   marked_by  = HLV bấm      ← từ đây biết AI DẠY BÉ NÀO
   marked_at  = lúc bấm
        ↓
Người bấm sau thấy: "HLV X đã tích lúc 17:12" (không phải lỗi)
        ↓
Cuối ngày → P4 chốt ngày
```

Bốn lớp chống tích trùng và cách xử lý tích nhầm: xem [[QT-02 - Điểm danh]].

### P4 — Chốt ngày

```
Điểm danh trong ngày
        ↓ mỗi dòng điểm danh sinh một biến động buổi (chờ xác nhận)
Danh sách chờ chốt
        ↓
Bước 1 — Số học viên đã học hôm nay: Quản lý BẮT BUỘC xác nhận trong ngày
        chưa xác nhận → cảnh báo `attendance_not_closed` tiếp tục nhắc, không tự tắt
        ↓
Bước 2 — Từng buổi của từng học viên: xem, sửa, hoặc bỏ qua — được phép để sang ngày sau
        ↓
Bấm chốt  →  xác nhận  →  trừ vào số buổi còn lại của gói
Không bấm →  hết ngày hệ thống trừ buổi dựa vào học viên đã được tích, KHÔNG trừ buổi của học viên chưa được tích
```

Chi tiết mức bắt buộc của từng bước: BR-15 (bước 1) và BR-33 (bước 2) ở [[QT-03 - Trừ buổi]].

## 4. Giai đoạn & lộ trình

| GĐ | Gồm | Chạy được việc gì |
|---|---|---|
| 1 | HT · HV · DH · TC | Thay được sổ giấy và nhóm chat: ghi danh, xếp lịch, điểm danh, trừ buổi, thu học phí |
| 2 | NS · LG | HLV tự chấm công, hệ thống tự tính lương gồm cả ca lẻ |
| 3 | TS · BH · BC | Hết thất lạc giày, có dashboard và cảnh báo |
| 4 | MK | Chỉ làm sau khi xem xét cách ghi nhận nguồn khách |

### Tiến độ đặc tả theo module

```dataview
TABLE WITHOUT ID
  ma AS "Mã",
  link(file.link, default(ten, file.name)) AS "Module",
  giai_doan AS "GĐ",
  trang_thai AS "Trạng thái"
FROM #module
SORT giai_doan ASC, ma ASC
```

## 5. Toàn bộ màn hình

Vì NT-1 là ưu tiên hàng đầu, mỗi màn hình có mục tiêu số thao tác cụ thể. Chi tiết từng màn hình nằm trong mục "Màn hình" của note module tương ứng.

| Mã  | Màn hình             | Tác nhân         | Module | GĐ  |
| --- | --------------------- | ---------------- | ------ | --- |
| S1  | Lịch tuần theo sân    | Admin            | DH     | 1   |
| S2  | Điểm danh tại sân     | HLV              | DH     | 1   |
| S3  | Chốt ngày             | Admin            | DH     | 1   |
| S4  | Hồ sơ học viên        | Admin, Phụ huynh | HV     | 1   |
| S5  | Ghi danh nhanh        | Admin            | HV     | 1   |
| S6  | Check-in / check-out  | HLV              | NS     | 2   |
| S7  | Bảng lương kỳ         | Admin            | LG     | 2   |
| S8  | Chuyển giao tài sản   | HLV              | TS     | 3   |
| S9  | Tôi đang giữ gì       | HLV              | TS     | 3   |
| S10 | Đơn đặt giày          | Admin            | BH     | 3   |
| S11 | Dashboard             | Admin            | BC     | 3   |

## 6. Rủi ro

| Rủi ro                                 | Ảnh hưởng                                        | Cách giảm                                                                                            |
| --------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| Sổ giao–nhận sai vì thao tác quá nhiều | Mất giày, quy trách nhiệm sai người              | Giao theo lô nhiều món một lần · người nhận xác nhận lại · màn hình S9 để HLV tự soát · nhắc định kỳ |
| Chốt ngày bị bỏ quên nhiều ngày        | Gói học sai số buổi, khó truy lại                | Cảnh báo ở BC, cho chốt bù ngày cũ, không khoá quá khứ cho tới khi chốt kỳ kế toán                   |
| Lịch đổi sát giờ                       | HLV tới nhầm sân, học viên tới lúc không có ai   | Thông báo đẩy ngay khi lịch đổi, cho cả HLV và phụ huynh                                             |
| Hai HLV cùng tích một bé               | Hai dòng điểm danh, trừ hai buổi của cùng một bé | Bốn lớp chống tích trùng ở [[QT-02 - Điểm danh]]                                                     |
| HLV tích nhầm bé của người khác        | Ghi sai người dạy, ảnh hưởng thống kê            | Nút *nhận lại*, có nhật ký thao tác; Admin duyệt lại cuối ngày                                       |
| HLV quên check-out                     | Giờ công sai, lương sai                          | Tự đóng theo giờ kết thúc buổi trong lịch, đánh dấu lệch để Admin soát                               |
| Học viên đổi loại hình giữa khoá       | Số buổi còn lại tính sai                         | Trừ theo loại thực tế dạy — xem [[QT-03 - Trừ buổi]]                                                 |

## 7. Chưa nằm trong phạm vi

- **Thiết kế database** (ERD, schema, kiểu dữ liệu từng cột) — xem `05-Thuc-the-du-lieu`, bạn tự làm sau khi phần này xong.
- Chưa chọn stack: framework, cơ sở dữ liệu, hạ tầng.
- Chưa thiết kế giao diện, mới dừng ở danh sách màn hình và mục tiêu thao tác.
- Chưa tích hợp VNPay sandbox, mới ghi nhận là hướng cho thanh toán QR.
- Chưa có chi phí thuê sân, để dành khi CLB mở rộng.

## Liên quan

- Module chi tiết: thư mục `01-Module/`
- Vai trò và quyền: thư mục `02-Vai-tro/`
- Quy tắc nghiệp vụ (BR-01..32): thư mục `03-Quy-tac-nghiep-vu/`
- Quyết định đã chốt: thư mục `04-Quyet-dinh/`
- Thực thể dữ liệu (chờ thiết kế): thư mục `05-Thuc-the-du-lieu/`
