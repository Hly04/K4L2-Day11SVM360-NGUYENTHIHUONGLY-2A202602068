# Error analysis card

## Zone × block

| zone | block | what | count |
|---|---|---|---:|
| center | B2 | BOX_GEOMETRY | 1 |
| center | B2 | STRUCTURE | 1 |
| center | B4 | MISSING | 1 |
| center | B4 | SPURIOUS | 5 |
| center | B4 | WRONG_CLASS | 1 |
| center | C0 | MISSING | 3 |
| edge | B4 | SPURIOUS | 1 |
| edge | C0 | MISSING | 1 |
| mid | B2 | STRUCTURE | 1 |
| mid | B4 | ATTRIBUTE | 2 |
| mid | B4 | IGNORE_SCOPE | 1 |
| mid | B4 | MISSING | 13 |
| mid | B4 | SPURIOUS | 11 |
| mid | C0 | MISSING | 2 |

## Top defects
- MISSING: 20 (ví dụ frame adasind_019560.jpg)
- SPURIOUS: 17 (ví dụ frame adasind_261480.jpg)
- STRUCTURE: 2 (ví dụ frame adasind_060000.jpg)

## Phân tích của bạn

Hai bảng trên do `python3 lab11.py card` tính từ `findings.csv`; chạy lại lệnh sẽ cập nhật bảng và giữ nguyên mục này. Viết cho lỗi nổi bật nhất, dẫn frame/`object_ref`.

- Nguyên nhân khả dĩ (`why`) và vì sao bạn nghĩ vậy: Lỗi nổi bật nhất là `MISSING` (20 ca) và `SPURIOUS` (17 ca), tập trung chủ yếu ở vùng `mid` của slice B4 (13 missing, 11 spurious). 
  + Đối với annotator (L): Lỗi `MISSING` (ví dụ `adasind_265065.jpg` các ca R1, R5, R8) xảy ra do mật độ phương tiện đông đúc và hiệu ứng méo quang học fisheye ở vùng `mid`, khiến annotator ước lượng nhầm kích thước chiều cao vật thể dưới ngưỡng $H=40$px hoặc bỏ sót xe máy bị che khuất một phần (`E1_annotator_error`).
  + Đối với model (M): Lỗi `SPURIOUS` (ví dụ `adasind_261480.jpg` các box M2, M5, M8) phát sinh do mô hình YOLO đóng băng được huấn luyện trên miền ảnh perspective phẳng thông thường, dẫn đến hiện tượng lệch miền (`E4_model_domain`), nhận diện nhầm bóng cây, mép tường nhà và biển báo cong ở rìa thành phương tiện.
- Cách sửa và ai nhận việc (`owner`): 
  + Đối với lỗi missing của annotator: `owner=annotator` tiếp nhận, tiến hành rà soát kỹ lưỡng vùng `mid` bằng checklist 9 mục, phóng to ảnh để đo đạc chính xác ngưỡng chiều cao $H \ge 40$px và kiểm tra các phương tiện bị che khuất (`occluded`).
  + Đối với lỗi spurious của model: `owner=ai_team` tiếp nhận, đề xuất fine-tune model trên tập dữ liệu fisheye chuyên dụng (ADASIND) với augmentation mô phỏng méo thấu kính và bổ sung nhãn vùng loại trừ (`ignore_region`) vào hàm mất mát để triệt tiêu false positive.
- Bằng chứng (ảnh trong `screenshots/`, dòng findings, rule): 
  + Ảnh minh chứng: `submission/screenshots/adasind_265065_escalation.jpg` và `submission/r3_diag/model_compare.html`.
  + Dòng findings: `round=r3_diag, slice=B4-mid, frame=adasind_265065.jpg, object_ref=R1+M3, cell=RM_noL, what=MISSING` và các dòng `cell=M_only, what=SPURIOUS`.
  + Luật áp dụng: Quy tắc R01 (ngưỡng $H=40$px trong vùng hợp lệ), R02 (vẽ trên ảnh fisheye gốc), và R05 (phân định `occluded` khi bị che).
