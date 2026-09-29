# Đề xuất gold set theo camera — tình huống giả lập

**Đầu bài:** 50.000 frame từ bốn camera SVM, ngân sách chọn 200 frame để review/gold. Đây là tình huống trên slide,
**không phải** 50.000 frame có trong repo. Phân bổ đúng 200 ở `45_sampling_plan.csv` cho bốn camera, mỗi camera có
normal và hard slice. “Gold set” ở đây là **kế hoạch tạo** reference sau kiểm chứng, không phải teaching reference
ADASIND hoặc nhãn bạn vừa vẽ. Nếu cần, dùng `notebooks/day11-svm360-colab.ipynb` để thử tổng phân bổ; notebook
không làm thay phần lý do.

| camera_id | Hard case cần chọn | Vì sao dễ sai | Annotation space / calibration cần giữ | Cách review trước khi gọi là gold |
|---|---|---|---|---|
| front | Xe cắt đầu cự ly gần (<3m), ngược sáng bình minh/hoàng hôn | Chói lóa làm mất biên dạng, méo quang học khi vật áp sát nắp capo | Ảnh fisheye gốc kết hợp ma trận intrinsic/extrinsic chiếu lên BEV | 2 annotator độc lập, kiểm tra đối soát IoU >= 0.7; Lead phân xử nếu bất đồng |
| rear | Xe bám đuôi sát cản sau, vật thể thấp trong bóng tối khi lùi | Góc nhìn chúc xuống làm biến dạng phối cảnh chiều cao; đèn xe sau gây chói | Hệ tọa độ camera sau cản xe, giữ nguyên viền kính và vùng loại trừ cản | Soát mù độc lập, đối chiếu dữ liệu cảm biến siêu âm sonar cho vật thể thấp |
| left | Xe máy vượt ở góc chết (seam Front-Left), bóng đổ dài vắt ngang | Méo rìa cực đại, chuyển động tương đối nhanh gây motion blur | Calibration camera gương trái, timestamp đồng bộ microsecond với camera trước | Dual review kết hợp kiểm tra đối chiếu hình chiếu trên mặt phẳng BEV 360 độ |
| right | Người đi bộ/xe đạp băng cắt góc cua bên phụ khi rẽ phải | Người bị mép ảnh cắt (`truncated`), biến dạng góc rộng thay đổi tỷ lệ | Hệ trục camera gương phải, bảo toàn `lens_border` và góc nhìn mép bánh trước | Review chéo bởi QA chuyên trách, đo đạc chính xác bằng thước đo pixel hình học |

- Khi nào cần refresh gold set (đổi camera, calibration hoặc rule):
  1. Thay đổi phần cứng camera (cảm biến, góc đặt, tiêu cự thấu kính fisheye).
  2. Hiệu chuẩn lại hệ thống (cập nhật ma trận intrinsic/extrinsic sau bảo dưỡng rig xe).
  3. Cập nhật Guideline (ví dụ nâng cấp rules v1.1.0 về phân định ranh giới vùng tư nhân).
  4. Trôi dạt miền dữ liệu ODD (chuyển địa bàn hoạt động từ đô thị sang cao tốc, thay đổi mùa mưa/khô).
- Một ca seam/cross-camera cần policy và evidence trước khi ghép hai box:
  Khi một xe máy nằm tại vùng giao thoa (seam) giữa camera trước và camera hông trái, nó xuất hiện đồng thời trên cả hai khung hình. Để quyết định gán cùng một Identity (nối track) thay vì coi là lỗi DUPLICATE hay 2 xe riêng biệt, cần đầy đủ bằng chứng kỹ thuật:
  1. Dấu thời gian chụp đồng bộ phần cứng (Hardware timestamp đồng bộ với độ trễ < 5ms).
  2. Ma trận hiệu chuẩn ngoại (Extrinsics) chuyển đổi 2 box về cùng một tọa độ 3D trên mặt đường với độ lệch tâm < 0.3m.
  3. Vector đặc trưng thị giác (Re-ID embedding) có độ tương đồng cosine > 0.85 và vector vận tốc chuyển động nhất quán.
- Vì sao peer agreement hoặc quality report trên ảnh một camera chưa chứng minh gold set đúng cho cả bốn camera:
  Sự đồng thuận trên ảnh một camera (như tập ADASIND một góc nhìn) chỉ đo đạc tính nhất quán nội tại (intra-camera) trong điều kiện quang học cục bộ. Nó hoàn toàn bỏ qua các lỗi sai lệch liên camera (inter-camera) nghiêm trọng như: sai lệch hiệu chuẩn hình học giữa 4 góc, mất đồng bộ thời gian (timestamp jitter), độ nhạy sáng không đồng đều giữa các cảm biến, và sự đứt gãy nhất quán đối tượng khi chuyển dịch qua các đường ghép mí (seam line) trong không gian nhìn toàn cảnh 360 độ.
