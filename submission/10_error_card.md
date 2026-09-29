# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B1 | MISSING | 3 |
| center | B1 | SPURIOUS | 6 |
| center | C0 | SPURIOUS | 2 |
| edge | B1 | IGNORE_SCOPE | 3 |
| edge | B1 | MISSING | 1 |
| edge | B1 | SPURIOUS | 1 |
| mid | B1 | MISSING | 6 |
| mid | B1 | SPURIOUS | 7 |
| mid | B1 | WRONG_CLASS | 1 |
| unknown | B1 | DUPLICATE | 1 |
| unknown | B1 | IGNORE_SCOPE | 2 |
| unknown | B1 | WRONG_CLASS | 1 |

## Top defects
- SPURIOUS: 16 (ví dụ frame adasind_019560.jpg)
- MISSING: 10 (ví dụ frame adasind_034080.jpg)
- IGNORE_SCOPE: 5 (ví dụ frame adasind_001320.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Lỗi nổi bật nhất là **SPURIOUS ở zone mid (7 ca) và center (6 ca)**, phần lớn là `M_only` — model tạo box mà cả nhãn của bạn (L) lẫn reference (R) đều không có, ví dụ frame `adasind_034080.jpg` có tới 6 box `M_only` (M7–M12, chủ yếu mid/center). Nguyên nhân khả dĩ (`why=E4_model_domain`): model YOLO26m đóng băng huấn luyện trên ảnh phẳng, chưa quen mật độ vật chồng lấn kiểu fisheye — đây là **giả thuyết**, cần thêm slice khác để xác nhận, không kết luận chỉ từ 3 frame.
- `MISSING` đứng thứ hai (10 ca), chủ yếu là model bỏ sót vật mà cả bạn và reference đều đồng ý có (`LR_noM`, 7/10 ca) — cùng nguyên nhân giả thuyết domain gap, `owner=ai_team`, `action=keep_with_reason` vì nhãn của bạn không sai. Số ít còn lại (`R_only`, 3 ca ở `adasind_014670.jpg` R5 và `adasind_034080.jpg` R9×2) là nhãn của bạn thật sự thiếu so với reference — `why=E1_annotator_error`, `owner=annotator`, `action=rework`.
- `IGNORE_SCOPE` (5 ca, chủ yếu `adasind_001320.jpg`) là lỗi rõ ràng nhất: thiếu polygon `ego_body` — `why=E1_annotator_error`, `severity=P0` (ảnh hưởng phạm vi so sánh, theo R10), `owner=annotator`, `action=rework`. Đây là nhóm ưu tiên sửa đầu tiên ở P5.
- Bằng chứng: `screenshots/adasind_001320.jpg_L2_ego.png`, `adasind_014670.jpg_L7_ego.png`, `adasind_034080.jpg_L9_ego.png` (3 ca ego_body); chi tiết từng dòng ở `submission/findings.csv` (cột `evidence`, `rule_id`); luật tương ứng ở `docs/02-rules-vi.md` (R01, R04, R07).
