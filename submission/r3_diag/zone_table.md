# Zone table (slice của bạn)

Lệnh `python3 lab11.py model` tự ghi bảng số (cùng cách đếm với `r1_craft/compare.md` và `model_compare.md`); chạy lại lệnh sẽ cập nhật bảng và giữ nguyên phần nhận xét. Bạn chỉ viết mục Nhận xét.

| Zone | n_ref | L missing | L spurious | M missing (`LR_noM` + `R_only`) | M thừa (`LM_noR` + `M_only`) | Lỗi L chính (`what`) |
|---|---:|---:|---:|---:|---:|---|
| center | 4 | 0 | 1 | 1 | 4 | — |
| mid | 13 | 6 | 2 | 3 | 7 | MISSING (5) |
| edge | 0 | 0 | 0 | 0 | 1 | — |

## Nhận xét

- Zone nào người (L) và model (M) gãy nhiều nhất, dẫn số ở bảng trên: Cả người gán nhãn (L) và mô hình (M) đều gãy nhiều nhất ở vùng **`mid`**. Tại vùng `mid`, người gán nhãn bỏ sót (missing) 6/13 đối tượng tham chiếu (n_ref = 13) và có 2 box thừa (spurious); lỗi chính là `MISSING` (chiếm 5 ca). Mô hình (M) cũng sai lệch nặng nhất ở vùng `mid` với 3 ca missing và tới 7 ca phát hiện thừa (`LM_noR` + `M_only`). Ở vùng `center`, độ chính xác tốt hơn rõ rệt (L không missing, chỉ thừa 1; M missing 1, thừa 4). Vùng `edge` không có đối tượng tham chiếu chuẩn (n_ref = 0), model nhận diện nhầm 1 box thừa do méo quang học ở rìa.
- Giả thuyết vì sao (méo fisheye, box lỏng, thiếu `ego_body`, ...) và giới hạn của slice ba frame: Vùng `mid` là nơi bắt đầu chuyển tiếp từ tâm ra rìa cong của thấu kính fisheye, hình ảnh các phương tiện bắt đầu bị biến dạng phi tuyến và méo góc. Mật độ phương tiện hỗn hợp dày đặc (xe máy, xe đạp, người đi bộ chen chúc) khiến người gán nhãn dễ bỏ sót các phương tiện bị che khuất một phần hoặc ước lượng sai ngưỡng chiều cao $H \ge 40$px dẫn đến bỏ sót. Đối với model (M), mô hình YOLO đóng băng được huấn luyện trên miền dữ liệu ảnh phẳng thông thường nên khi gặp độ méo fisheye và các vật thể lạ tại vùng `mid` dễ sinh ra hallucination/box ảo (7 box thừa). Giới hạn của slice 3 frame là dung lượng mẫu quá nhỏ, tập trung cục bộ vào một phân đoạn hành trình nên chưa phản ánh hết sự đa dạng thời tiết, ban đêm hoặc các góc camera khác (rear/side) quanh xe.
