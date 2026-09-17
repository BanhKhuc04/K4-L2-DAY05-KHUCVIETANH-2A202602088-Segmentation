# Báo cáo Day 5 — Segmentation

- Mã học viên theo lớp: **2A202602088**
- Ngày / CVAT local: **17/09/2026 / CVAT local**
- Công cụ đã dùng: **CVAT (Mask/Brush/Polygon), Google Colab, YOLO26x-sem và YOLO26x-seg để tạo pre-annotation; QC và sửa thủ công trên CVAT; export COCO 1.0 / Segmentation mask 1.1.**

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | |
| medium_instance | `medium_instance.zip` | 3 / 3 | |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | |
| cp1_holes | `cp1_holes.zip` | 1 / 1 | |
| cp2_slice | `cp2_slice.zip` | 1 / 1 | |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1 | |
| cp3_thin | `cp3_thin.zip` | 1 / 1 | |
| cp4_curb | `cp4_curb.zip` | 1 / 1 | |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1 | |
| **Tổng tối đa** | | | |

Tất cả các ảnh đã được kiểm tra lại trên CVAT, Save và export theo đúng format của từng task. Cột điểm để trống, không tự điền điểm.

## 2. Một quyết định trước khi dùng gợi ý

- Phần tôi tự vẽ thủ công trước khi dùng gợi ý: **Tôi tự vẽ vùng `road` (mặt đường) trong bài semantic, bám theo ranh giới phần mặt đường nhìn thấy trong ảnh.**
- Class và quy tắc tôi dùng để chọn biên: **Với `road`, tôi dựa vào ranh giới mặt đường thực tế, mép vỉa/lề và vùng xe chạy; không để mask ăn sang `sidewalk` hoặc các vùng nền khác.**
- Nếu dùng gợi ý sau đó: **Sau phần tự vẽ thủ công, tôi dùng gợi ý từ model để tăng tốc. Tôi vẫn kiểm tra lại mask trên CVAT, sửa các vùng ăn sang lớp khác, vùng biên chưa sát và các chỗ bị bỏ sót trước khi Save và export.**

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: **`easy_semantic` — ảnh `7ee6d192-89e2408b.jpg`, vùng taluy/lề hai bên mặt đường.**
- Lỗi thuộc loại: **sai lớp + biên.**
- Bằng chứng tôi nhìn thấy: **Pre-annotation tự động có một số vùng ở hai bên đường bị tràn hoặc nhầm giữa `building`, `sidewalk` và vùng nền thực tế; ranh mask không bám đúng cấu trúc nhìn thấy trong ảnh.**
- Quy tắc và hành động sửa: **Tôi kiểm tra lại theo ranh nhìn thấy và chức năng của vùng, xóa phần mask ăn sang vùng khác và chỉnh lại biên bằng công cụ mask/brush trong CVAT. Không giữ kết quả AI chỉ vì confidence/model dự đoán cao.**
- Sau sửa đã Save và export lại chưa? **Đã Save và export lại.**

Nếu đã xem Summary trên GitHub Actions hoặc tự chạy script: **chỉ dùng để kiểm tra cấu trúc/trạng thái; không tự ghi điểm, PASS hay bonus vào báo cáo.**

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `cp4_curb` — ranh bó vỉa giữa lòng đường và phần đi bộ | Có thể nhìn màu bề mặt và gán thành `road`, hoặc dựa vào chức năng để gán `sidewalk` | Bó vỉa/độ cao và chức năng sử dụng quan trọng hơn việc hai vùng có màu nhựa gần giống nhau | Tôi chọn `road` cho mặt xe chạy và `sidewalk` cho phần phía trên/sau bó vỉa |
| `cp2_slice` — các xe cùng lớp nằm sát nhau | Gộp nhiều xe thành một mask hoặc tách từng xe | Mỗi vật thể đếm được phải là một instance riêng; có ranh/khe nhìn thấy giữa các xe | Tôi tách từng xe thành object riêng, dù chúng cùng class và nằm rất sát nhau |
| `cp5_occlusion` — vật thể bị một vật khác che khuất | Tách phần nhìn thấy thành hai object hoặc coi là một object duy nhất | Vật bị che vẫn là một instance; chỉ gán phần thực sự nhìn thấy và không tự vẽ xuyên vật che | Tôi giữ một instance cho cùng vật thể và chỉ mask các phần nhìn thấy |
