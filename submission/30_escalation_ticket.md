# Escalation ticket

## Ticket 1

- **Frame:** `adasind_265065.jpg` (đối tượng tham chiếu `R7`)
- **Ảnh chụp:** `submission/screenshots/adasind_265065_escalation.jpg`
- **Expected impact:** Reference hiện tại gán box cho đối tượng nằm khuất sâu trong bóng râm ven đường, độ phân giải mờ nhòe không phân biệt được loại phương tiện. Việc này khiến annotator và mô hình đều bị tính lỗi MISSING sai thực tế, làm sai lệch chỉ số đánh giá chất lượng (quality metric) và gây nhiễu cho quá trình đào tạo mô hình tự hành.
- **Owner:** `guideline`
- **Recommendation:** Điều chỉnh nhãn đối tượng `R7` thành polygon `ignore_region` với thuộc tính `reason = unreadable`, hoặc xóa bỏ khỏi Ground Truth của tập kiểm chuẩn để tránh phạt oan người gán nhãn tuân thủ đúng quy tắc nhận diện.
