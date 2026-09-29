# Kế hoạch review từ lỗi quan sát được

Từ `findings.csv` và `zone_table.md`, chọn **hai lát cắt của bài ADASIND một camera** cần review trước. Bảng này
giải thích dữ liệu thật bạn vừa làm; nó không thay cho kế hoạch bốn camera giả lập ở `45_sampling_plan.csv`.

| Lát cắt / frame | Số ca và loại lỗi | Vì sao review trước | Bằng chứng cần giữ |
|---|---|---|---|
| `adasind_261480.jpg` (Vùng `mid` chuyển tiếp méo fisheye) | 8 ca `SPURIOUS` (model M) và 2 ca `ATTRIBUTE` (L) | Khu vực này model sinh ra nhiều box ảo nhất do lệch miền dữ liệu fisheye; đồng thời annotator dễ lẫn lộn giữa `occluded` và `truncated` khi xe ở vùng cong. | Báo cáo `r3_diag/model_compare.html` và danh sách findings dòng M2, M5, M8. |
| `adasind_265065.jpg` (Đô thị giao cắt phức tạp) | 5 ca `MISSING` (L) và 1 ca `WRONG_CLASS` (L) | Chứa nhiều phương tiện và người đi bộ bị bỏ sót do kích thước sát ngưỡng $H=40$px và lỗi gán nhầm ThreeWheeler thành Truck, đe dọa trực tiếp an toàn va chạm. | Báo cáo `submission/rework/delta.md` và ảnh `submission/screenshots/adasind_265065_escalation.jpg`. |

Giới hạn của kết luận từ ba frame ADASIND: Ba frame này chỉ thuộc về một camera đơn hướng phía trước, quay vào ban ngày trong điều kiện giao thông cụ thể tại Ấn Độ. Kết quả đo được trên 3 frame này không thể đại diện cho hiệu năng của toàn bộ hệ thống SVM 360 độ gồm 4 camera (vốn có các góc nhìn hông/sau với khoảng cách quan sát, góc mù, và đặc tính quang học rất khác biệt).

## Chuyển sang kế hoạch bốn camera giả lập

Cách soát độ phủ của 200 frame ở `45_sampling_plan.csv` (kể cả tránh đếm nhiều frame liền nhau trong cùng cảnh như nhiều ca độc lập), và vì sao kế hoạch đó chỉ giúp tìm ca cần soi, chưa đo được tỷ lệ lỗi:
- Để kiểm soát độ phủ, áp dụng chiến lược lấy mẫu phân tầng (stratified sampling) kết hợp giãn cách thời gian (temporal subsampling): chỉ chọn tối đa 1 frame trong mỗi cửa sổ thời gian 3-5 giây để tránh đếm trùng lặp các frame liền kề cùng cảnh tĩnh (autocorrelation). Phân bổ frame đồng đều qua các điều kiện: ban ngày, chạng vạng, ban đêm, ngược sáng, trời mưa và các khúc cua gấp cho cả 4 camera (front, rear, left, right).
- Kế hoạch 200 frame này được thiết kế có chủ đích nhằm tập trung vào các trường hợp khó (hard cases chiếm tới 50% = 100/200 frame) để phát hiện lỗi tiềm ẩn ở các góc biên (corner cases). Do tỷ lệ ca khó bị thổi phồng so với thực tế vận hành (trong 50.000 frame thông thường, ca khó chỉ chiếm khoảng 2-5%), nên tỷ lệ lỗi đo được trên 200 frame này chỉ mang tính chất định tính nhằm khoanh vùng lỗi cần sửa, không thể coi là tỷ lệ lỗi thực tế (true defect rate) của toàn hệ thống.
