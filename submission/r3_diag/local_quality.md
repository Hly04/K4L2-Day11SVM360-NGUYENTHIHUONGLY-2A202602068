# Đối chiếu chất lượng cục bộ — rectangle

Teaching reference, không phải gold set đã phê duyệt; không có điểm đạt tự động.
Nguồn: export r1_craft đã khóa SHA256 `4b638103229ff8b73e9c5c85f5b590098494f0c55ae61b23731e949d3b6dda68`; slice `B4-mid`.
Ghép hình học greedy một-một theo IoU ≥ 0.50, rồi so class; H ≥ 40 px.
Box trái nằm chủ yếu trong ignore_region reference không tính. Polygon, polyline, track không được chấm.
Đây là phép tính offline của lab, không phải báo cáo hay kết quả tương đương CVAT Premium.

Frame được tính: adasind_249480.jpg, adasind_261480.jpg, adasind_265065.jpg. Frame thiếu trong export: không.
TP=11; FP=3; FN=6; số lần đối chiếu=19; mean IoU của TP=0.859.

| Chỉ số | Micro | Macro | Nhãn thấp nhất |
|---|---:|---:|---:|
| accuracy | 0.579 | 0.905 | 0.789 |
| precision | 0.786 | 0.860 | 0.500 |
| recall | 0.647 | 0.700 | 0.200 |
| jaccard | 0.550 | 0.573 | 0.200 |
| dice | 0.710 | 0.693 | 0.333 |

| Nhãn | TP | FP | FN | Accuracy | Precision | Recall | Jaccard | Dice |
|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 1 | 1 | 0.895 | 0.800 | 0.800 | 0.667 | 0.800 |
| Car | 2 | 2 | 0 | 0.895 | 0.500 | 1.000 | 0.500 | 0.667 |
| Pedestrian | 1 | 0 | 4 | 0.789 | 1.000 | 0.200 | 0.200 | 0.333 |
| ThreeWheeler | 3 | 0 | 0 | 1.000 | 1.000 | 1.000 | 1.000 | 1.000 |
| Truck | 1 | 0 | 1 | 0.947 | 1.000 | 0.500 | 0.500 | 0.667 |

| Frame | TP | FP | FN | Accuracy | Precision | Recall |
|---|---:|---:|---:|---:|---:|---:|
| adasind_249480.jpg | 2 | 0 | 0 | 1.000 | 1.000 | 1.000 |
| adasind_261480.jpg | 7 | 2 | 0 | 0.778 | 0.778 | 1.000 |
| adasind_265065.jpg | 2 | 1 | 6 | 0.250 | 0.667 | 0.250 |

Confusion matrix: hàng = teaching reference; cột = export đã khóa.
`<missing>` là thiếu box; `<extra>` là box thừa. Xem `local_quality_confusion.csv`.

| Reference \ Export | Bike | Car | Pedestrian | ThreeWheeler | Truck | <missing> |
|---|---:|---:|---:|---:|---:|---:|
| Bike | 4 | 0 | 0 | 0 | 0 | 1 |
| Car | 0 | 2 | 0 | 0 | 0 | 0 |
| Pedestrian | 0 | 0 | 1 | 0 | 0 | 4 |
| ThreeWheeler | 0 | 0 | 0 | 3 | 0 | 0 |
| Truck | 0 | 1 | 0 | 0 | 1 | 0 |
| <extra> | 1 | 1 | 0 | 0 | 0 | 0 |

Chi tiết xung đột trong `local_quality_conflicts.csv`; dữ liệu máy đọc trong `local_quality.json`.
Mismatching label đóng góp một FP cho class vẽ và một FN cho class reference; attribute khác được báo riêng.
Micro accuracy đếm mỗi cặp ghép sai class là một lần đối chiếu; Jaccard đếm cả FP và FN.
Macro/worst bỏ nhãn không xuất hiện ở cả hai phía; chỉ số không có mẫu là N/A.
