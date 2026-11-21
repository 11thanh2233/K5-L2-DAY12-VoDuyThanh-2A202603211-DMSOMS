# Danh sách lớp v0 — Võ Duy Thành · G11

Viết tiếp từ slide 54 bài giảng. Mỗi lớp trả lời: **model cần nhận ra cái này để làm gì?**

## DMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng DMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | eye_state (từng mắt) | open / closed / unknown | Phát hiện buồn ngủ: đo PERCLOS, thời gian nhắm mắt liên tục | Có; mất khi đốm phản xạ LED trên mắt/kính (D06) hoặc tóc che (D05) |
| 2 | gaze_zone | road, mirror_l/r/c, cluster, center, lap, other | Cảnh báo mất tập trung: tính thời gian rời mắt khỏi đường | Có khi thấy mặt; quay hẳn đầu thì mất (D02: 12/13 điểm mặt bị che, gaze=other) |
| 3 | head_pose (yaw/pitch, suy từ face13) | góc độ | Hỗ trợ gaze khi không thấy mắt; phát hiện gục đầu | Có, 13 điểm mặt vẫn đặt được trên NIR (D01, D04, D06) |
| 4 | phone | held / mounted / lap | Cảnh báo dùng điện thoại khi lái (gọi điện, nhắn tin) | Một phần: máy tối khó tách khỏi tay; chỉ gán khi thấy cạnh máy (D04) |
| 5 | hand.on_wheel | yes / no / unknown | Kiểm tra tài xế sẵn sàng tiếp quản ở chế độ tự lái cấp 2/3 | Có nhưng vô lăng đen phản xạ kém, cần chỉnh contrast (D02) |
| 6 | glasses | none / clear / sun / unknown | Chọn chiến lược đọc mắt (kính gây phản xạ); không phạt nhầm người đeo kính | Có với kính trong; kính râm trên NIR có thể trông trong suốt |
| 7 | person.seatbelt (driver) | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây; đánh giá rủi ro khi va chạm | Có, đai đen trên áo sáng rất rõ (D01, D02, D06) |
| 8 | eating_drinking (đề xuất mới) | yes / no | Phát hiện ăn uống gây mất tập trung (D01 đang uống nước) | Có, chai nước phản xạ IR rõ |

## OMS (ít nhất 6 lớp)

| # | Lớp / thuộc tính | Giá trị | Để làm gì (chức năng OMS) | Nhìn thấy được trên IR? |
|---|---|---|---|---|
| 1 | person + seat | driver … rear_right | Đếm người theo ghế: nhắc thắt dây từng ghế, điều chỉnh túi khí | Có, hình dạng người rõ cả khi RGB tối (S07) |
| 2 | person.seatbelt | worn_correct / worn_incorrect / not_worn / unknown | Nhắc thắt dây ghế sau | Có; nhưng SVIRO không có ai thắt dây nên chưa học được worn_* |
| 3 | child_seat + occupied | no / unknown, theo ghế | Phát hiện trẻ bị bỏ quên; tắt túi khí ghế trước nếu có ghế trẻ em | Có (hình dạng); bên trong ghế bị chăn phủ thì không (S01, S08) |
| 4 | infant_seat + facing | rear / front, theo ghế | Tắt túi khí khi ghế em bé quay ngược ở ghế trước; nhắc quên trẻ | Có (hình dạng tay cầm, vỏ ghế) |
| 5 | child_in_seat (đề xuất mới) | có / không, theo ghế | Nhắc trẻ bị bỏ quên trong xe (CPD) — lý do chính để có OMS | Có (S03, S04, S07 thấy trẻ rõ trên IR-sim) |
| 6 | object + type | bag / box / cup / other, theo ghế | Nhắc quên đồ khi rời xe; không bật cảnh báo dây an toàn cho ghế chỉ có đồ | Có (hình dạng); mất màu nên khó tách túi/ba lô giống nhau |
| 7 | face (OMS) | box | Nhận diện người ngồi để cá nhân hoá (ghế, điều hoà) | Có khi quay về camera; không khi quay đi (S05) |

## 2 câu hỏi leo thang

1. Câu hỏi: Guideline nói bộ ảnh "không có trẻ em ngồi" và `child_seat.occupied` không có `yes`, nhưng bộ G11 có trẻ em ngồi trong ghế trẻ em và em bé trong nôi. Cần gán thế nào: thêm `occupied = yes` và box `person` cho trẻ, hay một lớp `child` riêng?
   Khung ví dụ: G11 S04_rgb / S04_ir (ghế phải xe), cũng gặp ở S02, S03, S07.
   Bạn tạm gán thế nào và vì sao: bản đầu ghi `occupied = unknown` vì schema chưa có `yes`. Sau khi lab coach bổ sung giá trị `yes` (đã sửa nhãn CVAT và `cabin_schema.py`), bản v2 ghi `occupied = yes` cho S03, S04, S07; vẫn **không** vẽ `person` cho trẻ vì guideline chưa có quy tắc, và `infant_seat` (S02) chưa có thuộc tính occupied.

2. Câu hỏi: Khi tài xế quay hẳn đầu đi (không thấy mắt lẫn mũi) thì không được vẽ `face`, nên mất luôn `gaze_zone`. Nhưng đây chính là ca mất tập trung nặng nhất mà DMS cần bắt. Có nên thêm thuộc tính trên `person`, ví dụ `head_turned = left / right / down / no`, để không mất thông tin này?
   Khung ví dụ: G11 D02 (tài xế quay đầu sang phải, gần như chỉ thấy gáy và tai).
   Bạn tạm gán thế nào và vì sao: theo review vẽ `face` và `face13` với 12/13 điểm bị che, `gaze_zone = other`; nhưng `other` lẫn ca "không chắc" với ca "quay hẳn đi", nên đề nghị thêm thuộc tính hướng đầu.
