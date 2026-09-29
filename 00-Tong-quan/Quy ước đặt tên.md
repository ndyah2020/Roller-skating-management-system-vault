---
loai: quy ước
tags:
  - tong-quan
---

# Quy ước đặt tên

Một chỗ duy nhất cho các quy ước dùng chung. Đừng tự chế giá trị mới ở note khác sửa ở đây rồi áp dụng lại.

## Tên file theo loại note

| Loại                                                    | Dạng tên file              | Ví dụ                                      |
| ------------------------------------------------------- | -------------------------- | ------------------------------------------ |
| Module                                                  | `MÃ - Tên module`          | `DH - Dạy học`                             |
| Vai trò                                                 | `Tên vai trò`              | `HLV`, `Phụ huynh - Học viên`              |
| Quy tắc (nhóm)                                          | `QT-xx - Tên nhóm quy tắc` | `QT-01 - Lớp và lịch`                      |
| Quyết định                                              | `QĐ-xx - Tiêu đề ngắn`     | `QĐ-01 - Phụ cấp ca lẻ là số tiền cố định` |
| Thực thể | `NN - tên_bảng_tiếng_anh` (NN = số thứ tự ưu tiên đọc, xem [[Sơ đồ dữ liệu]]) | `16 - session` |

Riêng note thực thể: mỗi note vẫn giữ tên tiếng Việt cũ trong frontmatter (`ten`) và trong `aliases`, nên link `[[Tên tiếng Việt cũ]]` viết ở bất kỳ note nào khác vẫn tự trỏ đúng — không cần sửa lại các link đã có.

Bên trong mỗi nhóm quy tắc, từng quy tắc con giữ mã gốc `BR-01`..`BR-32` để tham chiếu khi viết code và viết test — không đổi số.

## Giá trị `trang_thai` (module)

`Chưa làm` · `Đang đặc tả` · `Đã đặc tả` · `Đang code` · `Xong`

## Giá trị `giai_doan`

`1` — nền tảng (HT · HV · DH · TC) · `2` — nhân sự & lương (NS · LG) · `3` — tài sản, bán hàng, báo cáo (TS · BH · BC) · `4` — marketing (MK)

## Mã dùng trong toàn vault

- `NT-x` — nguyên tắc thiết kế (5 cái, xem note Tổng quan)
- `Px` — quy trình nghiệp vụ cốt lõi (P1–P7, xem note Tổng quan)
- `Sx` — màn hình (S1–S11, mỗi màn hình nằm trong bảng "Màn hình" của note module chủ)
- `BR-xx` — quy tắc nghiệp vụ (BR-01–BR-32, gộp theo 6 nhóm trong `03-Quy-tac-nghiep-vu`)
- `QĐ-xx` — quyết định đã chốt (`04-Quyet-dinh`)

## Cột kỹ thuật dùng chung mọi bảng *(áp dụng khi thiết kế database, chưa làm ở giai đoạn này)*

`id` · `created_at` · `updated_at` · `created_by` · `updated_by` · `is_active`

- Cột kết thúc bằng `_by` trỏ tới người bấm nút (user.id).
- Cột kết thúc bằng `_id` trỏ tới bảng cùng tên, trừ khi có tiền tố nói rõ hơn.
- Không xoá cứng — chỉ đánh dấu `is_active = false`.
