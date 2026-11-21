# Tự đánh giá — Võ Duy Thành · G11

## 1. Tự rà soát 4 cảnh

Nghỉ tay 1 phút, mở lại bài của mình, chọn 4 ảnh (2 S##, 2 D##), đọc như người lạ.

Loại lỗi: `seat_flip`, `wrong_class`, `guess`, `box`, `missing`, `extra`, `lr_flip` (trái/phải mặt), `flag` (thiếu cờ điểm), `ok`.

| Ảnh | Đối tượng | Loại lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|
| S05_rgb | person ghế phải xe (trái ảnh) | box | Box `person` ban đầu cắt mất bàn chân ở góc dưới trái (xtl 140) | Có, xtl = 113 |
| S06_rgb | infant_seat ghế phải xe | box | Đáy box lỏng ~20 px dưới vỏ nôi | Có, ybr 625 → 605 |
| D03 | person (driver) | guess | Đổi `not_worn` → `unknown` vì đoán "đang thắt dở" từ vị trí tay; review chỉ ra thân áo thấy trọn, không có dây | Có, v2 trả lại `not_worn` (§4.7) |
| D05 | person (driver) | guess | Ghi `not_worn` rồi `unknown`; review chỉ ra đoạn dây chéo xuống khoá hông phải thấy được | Có, v2 sửa `worn_correct` (§4.7) |

Các ca kiểm lại và giữ nguyên (`ok`): ghế OMS gọi theo xe ở cả 8 cảnh (trái ảnh = `rear_right`); `face13` của D01/D04/D06 có `left_eye_*` y lớn hơn `right_eye_*`; sau review thêm `face` cho S05 và D02.

## 2. Lỗi mình lặp lại nhiều nhất hôm nay

Lỗi: kết luận từ suy diễn hoặc từ ảnh chưa phóng to, thay vì từ cái thật sự nhìn thấy — `seatbelt` D03 (suy "đang thắt dở" từ vị trí tay dù thân áo thấy rõ không có dây), `seatbelt` D05 (bỏ qua đoạn dây xuống khoá hông), `face` S05 (không phóng to nên bỏ sót mắt).

Vì sao mình mắc: khi không chắc, mình vội chọn `unknown` hoặc bỏ đối tượng để "an toàn", nhưng `unknown` chỉ đúng khi đã nhìn kỹ mà vẫn không thấy. Chọn `unknown` hay bỏ box mà chưa nhìn kỹ cũng là đoán.

Lần sau tránh bằng cách: với dây an toàn, dò cả đường vai → hông đối diện **và** vùng khoá; thấy áo trọn mà không có đai → `not_worn`, thấy một đoạn đai → `worn_*`, bị che hết → `unknown`. Trước khi bỏ một đối tượng hoặc ghi `unknown`, phóng to và tăng contrast một lần.

## 3. Ghi chú về disagreement_log

Dòng `S05 face box` (reviewer đúng, mình sai). Mình kết luận "không thấy mắt lẫn mũi" chỉ từ ảnh cỡ gốc; phóng to thì thấy rõ một mắt và miệng dưới mái tóc. Bài học: trước khi **bỏ** một đối tượng (không vẽ box) phải phóng to kiểm như khi vẽ, vì bỏ sót khó bị script phát hiện hơn vẽ sai.
