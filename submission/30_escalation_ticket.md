# Escalation ticket

## Ticket 1

- **Frame:** `adasind_014670.jpg`, object `L1` (+ `M3` trong so sánh 3 nguồn)
- **Ảnh chụp:** `submission/screenshots/adasind_014670.jpg_L1_bus.png`
- **Expected impact:** Ca này xuất hiện ở hai chỗ khác nhau trong quy trình so sánh — trong `compare r1_craft` (chỉ so L với R) nó bị gắn `WRONG_CLASS` (L1 khớp R5 nhưng khác class, QA đề nghị đổi Bus→Truck); nhưng trong so sánh 3 nguồn (`model r3_diag`) nó lại hiện `LM_noR` (L1 khớp M3, không khớp được với R nào). Nếu không làm rõ, `findings.csv` có nguy cơ đếm trùng 1 lỗi thành 2 dòng khác bản chất, ảnh hưởng số liệu zone_table và kết luận về model.
- **Owner:** `qa` (cần người soát xem lại thuật toán ghép hoặc chính ảnh gốc để phân xử)
- **Recommendation:** Mở `submission/r2_qa/qa_overlay.html` và `submission/r3_diag/model_compare.html` cùng lúc, xác nhận L1/R5/M3 có phải cùng một vị trí vật lý không. Nếu đúng — đây chỉ là hệ quả của lỗi `WRONG_CLASS` (ghép 3 nguồn yêu cầu đúng class nên bị tính là "reference không có"), không cần coi là lỗi reference riêng; chỉ cần sửa class ở P5 là tự hết. Nếu không trùng vị trí — đây là 2 lỗi độc lập, cần đánh giá `E0_reference_defect` riêng cho ca `LM_noR`.
