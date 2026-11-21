# Tự đánh giá — Võ Duy Thanh · G11

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S05_rgb | person ghế phải xe (trái ảnh) | box | Box `person` ban đầu cắt mất bàn chân ở góc dưới trái (xtl 140) | Có, xtl = 113 |
| S06_rgb | infant_seat ghế phải xe | box | Đáy box lỏng ~20 px dưới vỏ nôi | Có, ybr 625 → 605 |
| D03 | person (driver) | guess | Đổi `not_worn` → `unknown` vì đoán "đang thắt dở" từ vị trí tay; review chỉ ra thân áo thấy trọn, không có dây | Có, v2 trả lại `not_worn` (§4.7) |
| D05 | person (driver) | guess | Ghi `not_worn` rồi `unknown`; review chỉ ra đoạn dây chéo xuống khoá hông phải thấy được | Có, v2 sửa `worn_correct` (§4.7) |

Các ca kiểm lại và giữ nguyên (`ok`): ghế OMS gọi theo xe ở cả 8 cảnh (trái ảnh = `rear_right`); `face13` của D01/D04/D06 có `left_eye_*` y lớn hơn `right_eye_*`; D02 không vẽ `face` vì chỉ thấy gáy.

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: `guess` ở `seatbelt` — ghi `not_worn` khi chỉ là *không thấy dây* (D03, D05).

Vì sao mình mắc: ở 4 ảnh D còn lại dây an toàn đen rất nổi trên áo sáng, nên khi không thấy dải đen mình mặc định "không đeo". Thực ra "không thấy" (tóc che, tay che, đang thắt) khác với "thấy rõ không có".

Lần sau tránh bằng cách: trước khi chọn `not_worn`, chỉ được vào vùng vai → hông đối diện và tự hỏi "mình có thấy **áo** ở đúng đường đó không?". Thấy áo mà không có đai → `not_worn`; bị che → `unknown`.

## 3. Ghi chú về disagreement_log

Dòng `S05 face box` (peer đúng, mình sai). Mình kết luận "không thấy mắt lẫn mũi" chỉ từ ảnh cỡ gốc; phóng to thì thấy rõ một mắt và miệng dưới mái tóc. Bài học: trước khi **bỏ** một đối tượng (không vẽ box) phải phóng to kiểm như khi vẽ, vì bỏ sót khó bị script phát hiện hơn vẽ sai.
