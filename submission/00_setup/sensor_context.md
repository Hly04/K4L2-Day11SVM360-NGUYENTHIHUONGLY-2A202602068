# Sensor context

- Rig: Camera fisheye góc siêu rộng gắn phía trước xe (khu vực kính lái/mui xe hoặc cản trước), hướng nhìn thẳng về phía trước theo chiều di chuyển của xe trong giao thông đô thị ADASIND.
- `ego_body`: Thân xe ego xuất hiện rõ ở phần đáy/cạnh dưới của khung hình (phần nắp capo/mũi xe nhìn thấy được từ góc đặt camera). Cần vẽ polygon `ego_body` phủ kín vùng này ở các frame có xuất hiện thân xe ego.
- Vòng kính (lens circle): Vòng tròn quang học của ống kính mắt cá (fisheye) nằm ở trung tâm ảnh, chiếm phần lớn khung hình (khoảng 80-85% diện tích). Bốn góc và hai bên viền ngoài của vòng kính là các dải đen không có thông tin hình ảnh thực, được định nghĩa bởi `lens_border`.
