# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_034080.jpg`, zone mid/center | 6 `M_only` (SPURIOUS) + 1 `R_only` (MISSING) + 1 `DUPLICATE` (QA) | Frame đông vật nhất trong slice (nhiều box chồng lấn), tập trung hầu hết lỗi model thừa và ca `DUPLICATE` — rủi ro cao nếu dùng để đánh giá model | `screenshots/adasind_034080.jpg_*`, dòng `findings.csv` ứng với M7–M12, R9 |
| `adasind_001320.jpg`, zone edge | 3 `IGNORE_SCOPE` (thiếu `ego_body`) mức P0 | Lỗi phạm vi (R10) làm sai lệch toàn bộ phép so sánh của frame này, cần sửa trước khi tin bất kỳ số nào khác của frame | `screenshots/adasind_001320.jpg_L2_ego.png`, dòng findings mức P0 |

Giới hạn của kết luận từ ba frame ADASIND: 3 frame quá ít để tách "lỗi hệ thống của model" khỏi "đặc điểm riêng của từng frame" (frame `034080` đông vật hơn hẳn 2 frame còn lại), và không đại diện cho 4 camera SVM thật — chỉ một camera fisheye, không có seam/overlap giữa các camera.

## Chuyển sang kế hoạch bốn camera giả lập

Với 200 frame ở `45_sampling_plan.csv`, để tránh đếm nhiều frame liền kề trong cùng một cảnh (vd. cùng một xe đi qua 5 frame liên tiếp) như 5 ca lỗi độc lập, nên lấy mẫu **cách quãng theo thời gian** (vd. mỗi N giây/frame) thay vì lấy nguyên một đoạn video liên tục, và khi review chỉ tính 1 "sự kiện" cho một vật xuất hiện xuyên suốt nhiều frame liền nhau. Kế hoạch 200 frame (chia theo front/rear/left/right × normal/hard) chỉ giúp **tìm ra** ca khó cần soi kỹ ở từng camera — nó không tự động đo được tỷ lệ lỗi thật của hệ thống, vì 200 frame là mẫu chọn có chủ đích (thiên về ca khó), không phải mẫu ngẫu nhiên đại diện cho toàn bộ luồng video; muốn đo tỷ lệ lỗi cần một tập gold set riêng được lấy mẫu ngẫu nhiên/đại diện, như nêu ở `46_gold_set_plan.md`.
