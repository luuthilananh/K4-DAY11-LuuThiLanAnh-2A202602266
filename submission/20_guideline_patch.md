# Guideline patch

- **Rule mới đề xuất:** Khi hai box cùng class `Bike` chồng lấn gần như hoàn toàn (IoU cao, tâm box lệch <10% chiều rộng vật), chỉ giữ **một** box nếu không phân biệt được hai vật riêng biệt trên ảnh gốc; chỉ tách thành hai box khi thấy rõ hai bánh sau/hai người lái khác nhau. Ghi rõ lý do giữ 1 hay 2 box vào `note` của `findings.csv`.
- **Áp dụng cho:** class `Bike`, đặc biệt zone `mid`/`edge` nơi góc nhìn fisheye dễ làm một xe trông giống hai vật chồng nhau (rider + xe tách hình do méo ống kính).
- **Vì sao luật hiện tại không đủ:** `docs/02-rules-vi.md` (R03) chỉ nêu luật rider-ngồi-trên-xe = 1 box `Bike`, và rider-dắt-xe = `Pedestrian` + `Bike` tách riêng, nhưng không nói rõ cách xử lý khi **hai box cùng class `Bike` chồng lấn** do góc nhìn — ca thực tế gặp ở `adasind_034080.jpg` (L1, L2 — xem `screenshots/adasind_034080.jpg_L1_L2_bike_overlap.png`, được QA gắn cờ `DUPLICATE` ở `findings.csv`) không có luật rõ ràng để quyết định giữ 1 hay 2 box.
- **`rules_version` mới:** v1.0.0 → v1.1.0
- **Hiệu lực từ:** áp dụng từ round `rework` trở đi (ca `adasind_034080.jpg` L1/L2 là ca đầu tiên áp dụng luật mới này).
