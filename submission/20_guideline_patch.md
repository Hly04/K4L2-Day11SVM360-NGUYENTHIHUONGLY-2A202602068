# Guideline patch

- **Rule mới đề xuất:** R11 — Quy ước loại trừ phương tiện tĩnh trong không gian nhà dân / cửa hàng tư nhân (Off-road / Indoor Private Property). Các phương tiện (`Bike`, `Car`) hoặc người đứng sâu trong hiên nhà, trong quán xá hoặc sau hàng rào/cửa kính không tiếp giáp trực tiếp mặt đường lưu thông thì không gán nhãn Bounding Box, trừ khi một phần phương tiện nhô ra lòng đường/vỉa hè gây cản trở di chuyển.
- **Áp dụng cho:** Tất cả các lớp đối tượng phương tiện (`Bike`, `Car`, `ThreeWheeler`, `Pedestrian`) ở khu vực ranh giới mép đường và nhà dân.
- **Vì sao luật hiện tại (`docs/02-rules-vi.md`) không đủ:** Quy tắc R01 hiện tại chỉ quy định ngưỡng kích thước $H \ge 40$px trong "vùng hợp lệ" nhưng chưa định nghĩa ranh giới vật lý cụ thể của vùng hợp lệ khi camera fisheye quan sát thấy các phương tiện cất trong nhà dân hoặc cửa hàng hai bên đường. Điều này dẫn đến tranh cãi giữa annotator và reviewer về việc có gán nhãn các xe máy dựng trong hiên nhà hay không.
- **`rules_version` mới:** v1.1.0
- **Hiệu lực từ:** Áp dụng thống nhất từ vòng r1_craft của các đợt gán nhãn tiếp theo.
