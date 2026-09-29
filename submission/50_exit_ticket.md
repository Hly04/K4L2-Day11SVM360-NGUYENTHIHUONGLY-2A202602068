# Exit ticket

Đọc `docs/10-svm360-reading-vi.md` trước khi trả lời câu 1–2. Các câu về zone, `why`, rework, parking và sampling
đã nằm trong file tương ứng nên không hỏi lại ở đây.

1. Một vật ở vùng seam giữa hai camera thật xuất hiện với hai box khác nhau: đó là lỗi `DUPLICATE` hay cần một quy tắc riêng? Vì sao?
   - Đây KHÔNG phải là lỗi `DUPLICATE`, mà là hiện tượng quang học tự nhiên của hệ thống SVM đa camera có vùng quan sát chồng lấn (overlapping FOV) tại các góc xe, và bắt buộc cần một quy tắc riêng (Cross-Camera Seam Policy).
   - Vì mỗi camera ghi nhận hình ảnh vật thể từ một góc phối cảnh độc lập với độ méo thấu kính khác nhau; ở mức độ ảnh 2D của từng sensor, cả 2 box đều hoàn toàn hợp lệ và phản ánh đúng tín hiệu photon thu được. Chỉ khi chiếu lên không gian 3D / mặt phẳng Bird's-Eye-View (BEV) có ma trận hiệu chuẩn (calibration), hai box này mới được hợp nhất thành một thực thể duy nhất. Nếu coi là DUPLICATE ở mức 2D và xóa đi 1 box sẽ làm mất thông tin biên dạng cục bộ quan trọng của camera đó.

2. Một vật đi qua nhiều frame trên cùng camera: khi nào giữ cùng track ID, khi nào thêm keyframe hoặc trạng thái Outside? Nêu bằng chứng sẽ cần trước khi nối track qua hai camera.
   - Trong cùng 1 camera: Giữ cùng track ID khi vật thể di chuyển liên tục và duy trì được tính liên tục về hình dạng/quỹ đạo (kể cả khi bị che khuất ngắn hạn). Thêm keyframe khi đối tượng thay đổi đột ngột về hướng chuyển động, tốc độ hoặc hình học (do méo fisheye khi di chuyển từ center ra edge). Đặt trạng thái `Outside` ngay khi vật thể đi ra ngoài vành kính quang học hoặc bị che khuất hoàn toàn kéo dài, nhằm ngăn chặn bộ nội suy (interpolator) vẽ ra các box ảo không có thật trên đường đi.
   - Bằng chứng bắt buộc trước khi nối track qua hai camera: Cần có (1) Dấu thời gian chụp đồng bộ phần cứng (hardware timestamp sync < 5ms); (2) Ma trận hiệu chuẩn ngoại (Extrinsic calibration) chính xác giữa 2 camera; (3) Tính liên tục của vector vận tốc và quỹ đạo 3D (vật thể không dịch chuyển tức thời với vận tốc phi vật lý); (4) Vector đặc trưng thị giác (Visual Re-ID embedding) có độ tương đồng cao.

3. Nhìn lại cả buổi: một chỗ bạn tin nhãn mình đúng nhưng reference hoặc người soát nghĩ khác (dẫn frame/`object_ref`), bạn đã xử lý thế nào, và nếu làm lại slice này bạn sẽ đổi gì trong cách làm?
   - Ca cụ thể: Tại frame `adasind_265065.jpg` đối tượng `R7`, teaching reference gán 1 box cho vật thể nằm chìm sâu trong bóng râm ven đường, mờ nhòe không rõ hình dạng phương tiện, trong khi mình không gán box vì tin rằng vật thể vi phạm tiêu chí nhận diện rõ ràng. Mình đã xử lý một cách minh bạch: không sửa ép nhãn sai thực tế mà ghi nhận vào `submission/findings.csv` với mã `why = E0_reference_defect`, tạo `Escalation Ticket` đề xuất chuyển sang polygon `ignore_region` (`unreadable`) và ghi chú rõ ràng trong Decision Log.
   - Nếu làm lại slice này: Mình sẽ chú trọng hơn vào vùng chuyển tiếp `mid` ngay từ khâu gán nhãn thô, sử dụng công cụ phóng to để kiểm tra kỹ các phương tiện bị che khuất (`occluded`) và soát kỹ từng mục trong checklist 9 bước trước khi xuất bản nháp, giúp giảm thiểu số ca missing ngay từ vòng r1_craft.
