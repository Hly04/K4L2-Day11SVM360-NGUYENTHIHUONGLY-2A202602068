# Quan sát vạch ô đỗ

- Hai vạch `parking_line` đã vẽ (mô tả vị trí trong ảnh): Vạch phân chia ô đỗ rõ nét ở tiền cảnh bên phải (khoảng từ x=699, y=623 kéo dài đến x=960, y=684) và vạch ở tiền cảnh trung tâm (từ x=406, y=652 đến x=531, y=719). Cả hai vạch này đều có vai trò trực tiếp phân tách ranh giới giữa hai ô đỗ xe liền kề.
- Một vạch/dấu sơn hoặc biên **không** vẽ, và vì sao: Không vẽ đường viền mép vỉa hè (curb) và các vạch sơn dẫn hướng xe chạy ở khu vực xa phía trên, vì chúng đóng vai trò chỉ dẫn làn đường di chuyển/phân luồng giao thông nội bộ bãi xe, không phải vạch phân chia ranh giới ô đỗ riêng lẻ.
- Polygon `free_space` dừng ở đâu; có phần bị che nào không: Polygon `free_space` phủ phần mặt đường nhựa trống nhìn thấy được của lối lưu thông chính giữa các dãy đỗ; dừng chính xác tại ranh giới mép các ô đỗ xe và mép thân xe đỗ ở xa; không bao gồm vỉa hè, cây cỏ hay các vùng bị che khuất.
- Ca chưa chắc cần hỏi người soát (nếu không có, ghi “không có”): Một số đoạn vạch đỗ ở hậu cảnh xa bị mờ và đứt đoạn do góc chụp nghiêng và độ phân giải, cần người soát xác nhận xem có nên nối dài hay ngắt đoạn theo phần nhìn thấy.
