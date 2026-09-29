# Rework delta

| zone | matched before | matched after | missing before | missing after | spurious before | spurious after |
|---|---:|---:|---:|---:|---:|---:|
| center | 6 | 6 | 1 | 1 | 1 | 1 |
| mid | 8 | 8 | 1 | 1 | 1 | 2 |
| edge | 4 | 4 | 0 | 0 | 0 | 0 |

## Findings action=rework
- adasind_001320.jpg L2 IGNORE_SCOPE: đã sửa
- adasind_014670.jpg L7 IGNORE_SCOPE: đã sửa
- adasind_034080.jpg L9 IGNORE_SCOPE: đã sửa
- adasind_014670.jpg L1 WRONG_CLASS: chưa sửa
- adasind_034080.jpg L2 DUPLICATE: đã sửa
- adasind_001320.jpg L1 IGNORE_SCOPE: đã sửa
- adasind_034080.jpg L9 IGNORE_SCOPE: đã sửa
- adasind_014670.jpg L1+R5 WRONG_CLASS: chưa sửa
- adasind_014670.jpg L2 SPURIOUS: chưa sửa
- adasind_034080.jpg R9 MISSING: chưa sửa
- adasind_014670.jpg L2 SPURIOUS: chưa sửa
- adasind_014670.jpg R5 MISSING: chưa sửa
- adasind_034080.jpg R9 MISSING: chưa sửa

## Nhận xét

- Đã sửa đủ 3 ca mức P0 (thiếu `ego_body`, ảnh hưởng phạm vi so sánh của cả frame theo R10) và 1 ca P1 (`DUPLICATE` xe máy ở `adasind_034080.jpg`). Các ca P1 còn lại (`WRONG_CLASS` bus/truck, `SPURIOUS` L2, `MISSING` R5/R9) **chưa sửa** trong vòng rework này — ưu tiên đã dồn cho các ca P0 làm sai lệch cả phép so sánh trước.
- Số **spurious ở zone `mid` tăng từ 1 lên 2** sau rework — không cải thiện toàn bộ, dù các ca P0 đã sửa đúng. Nguyên nhân khả dĩ: sau khi xóa 1 trong 2 box `DUPLICATE` (xe máy chồng lấn, zone mid), box còn lại có thể không khớp hình học đủ tốt với reference (IoU thấp hơn ngưỡng), khiến nó bị tính là `spurious` mới thay vì `matched`. Đây là giả thuyết dựa trên vị trí ca đã sửa, chưa xác nhận trực tiếp trên overlay — cần xem `qa_overlay.html`/`compare.html` bản mới để kết luận chắc chắn. Giữ nguyên số liệu thật, không sửa tay báo cáo.
