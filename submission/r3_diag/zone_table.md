# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 7 | 1 | 1 | 2 | 4 | SPURIOUS (1) |
| mid | 9 | 1 | 1 | 6 | 7 | WRONG_CLASS (1) |
| edge | 4 | 0 | 0 | 1 | 1 | — |

## Nhận xét

- Zone gãy nhiều nhất là **mid**: model bỏ sót 6/9 vật đúng (`LR_noM`+`R_only` = 6) và tạo thừa 7 box (`LM_noR`+`M_only` = 7) — cao nhất trong ba zone. Zone `center` đứng sau với 2 missing / 4 thừa. Zone `edge` gần như sạch với model (0 missing, 1 thừa), dù đây là nơi méo fisheye mạnh nhất — ngược với kỳ vọng ban đầu.
- Giả thuyết: phần lớn lỗi model không nằm ở méo rìa (edge) như dự đoán, mà tập trung ở **mid** — có thể do mật độ vật cao hơn ở mid (nhiều xe/người chồng lấn ở frame `adasind_034080.jpg`, 6/6 box `M_only` của frame này đều ở zone mid/center) khiến model nhầm hoặc tạo box trùng. Một phần khác có thể do model gốc (YOLO26m) không được huấn luyện để nhận `ego_body`/vật bị khuất một phần bởi vòng kính, nên vừa bỏ sót (thấy 1 phần thì bỏ) vừa tạo box thừa (nhận nhầm phản chiếu/bóng làm vật). Đây là giả thuyết dựa trên 3 frame, **chưa đủ bằng chứng để kết luận chắc chắn** — cần thêm slice khác để kiểm tra zone `mid` có luôn là điểm yếu của model hay đây là đặc thù ngẫu nhiên của 3 frame này.
- Giới hạn: slice 3 frame quá nhỏ để tách rời "lỗi model hệ thống" khỏi "đặc điểm riêng của frame" (vd. `adasind_034080.jpg` đông vật hơn 2 frame còn lại nên đóng góp phần lớn số `M_only`).
