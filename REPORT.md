# Báo cáo Day 5 — Segmentation Lab

- Mã học viên theo lớp: 2A202602175
- Ngày / CVAT local: 17/09/2026 / CVAT local tại http://localhost:8080
- Công cụ đã dùng: EoMT-DINOv3 automatic annotation và Brush/Mask để kiểm tra, sửa mask

## 1. Bài đã nộp

Các ZIP dưới đây là các file đã export từ CVAT. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | `easy_semantic.zip` | 3 / 3 | 20 |
| medium_instance | `medium_instance.zip` | 3 / 3; export cần sửa label | 32 |
| hard_panoptic | `hard_panoptic.zip` | 2 / 2 | 30 |
| cp1_holes | `cp1_holes.zip` | 1 / 1; export sai format, cần COCO 1.0 | 3 |
| cp2_slice | `cp2_slice.zip` | 1 / 1; export sai format, cần COCO 1.0 | 3 |
| cp5_occlusion | `cp5_occlusion.zip` | 1 / 1; export sai format, cần COCO 1.0 | 3 |
| cp3_thin | `cp3_thin.zip` | 0 / 1; ZIP chưa có mask semantic hợp lệ | 3 |
| cp4_curb | `cp4_curb.zip` | 1 / 1; export sai format, cần Segmentation mask 1.1 | 3 |
| cp6_coverage | `cp6_coverage.zip` | 1 / 1; export sai format, cần Segmentation mask 1.1 | 3 |
| **Tổng tối đa** | | | **100** |

## 2. Một quyết định trước khi dùng gợi ý

Phần này ghi lại quyết định liên quan đến object đầu tiên ở `medium_instance`.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: chưa ghi lại chính xác tên ảnh/vị trí trong lúc làm; cần bổ sung trước khi nộp chính thức.
- Class và quy tắc tôi dùng để chọn biên: chọn một object thuộc class của task, chỉ gán phần nhìn thấy và dừng ở biên vật che; cần bổ sung class/vị trí cụ thể.
- Nếu dùng gợi ý sau đó: đã dùng EoMT-DINOv3 để đề xuất mask, nhưng cần ghi rõ vùng đã sửa/giữ sau khi đối chiếu ảnh.
- Nếu không dùng gợi ý: không áp dụng cho object đầu tiên; object đầu tiên cần được tự kiểm trước khi chấp nhận đề xuất tự động.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `medium_instance`, trong `annotations/instances_default.json`.
- Lỗi thuộc loại: sai lớp.
- Bằng chứng tôi nhìn thấy: ZIP có các category `building`, `road`, `sidewalk`, `sky`, `traffic light`, `vegetation`, không thuộc danh sách sáu class của `medium_instance`.
- Quy tắc và hành động sửa: cần mở lại task Medium, xóa hoặc sửa các label không thuộc task, giữ mỗi person/bicycle/car/motorcycle/bus/truck là một instance riêng, rồi Save và export lại COCO 1.0.
- Sau sửa đã Save và export lại chưa? Chưa; ZIP hiện tại đã được upload để ghi nhận trạng thái, cần thay bằng bản export sau khi sửa.

Kết quả tự đánh giá: chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Các ca dưới đây là những vùng cần kiểm tra theo quy tắc của từng task.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `cp2_slice`, ảnh `000000017627.jpg` | Hai xe sát nhau là một vùng hay hai instance | Quy tắc checkpoint yêu cầu hai vật cùng class sát nhau phải là hai instance riêng | Chọn hai mask riêng; cần kiểm lại khe giữa hai xe trước khi export COCO 1.0 |
| `cp5_occlusion`, ảnh `000000336232.jpg` | Hai phần nhìn thấy của vật bị che là hai vật hay một vật | Vật bị che vẫn giữ một instance nếu hai phần thuộc cùng vật | Chọn một instance cho vật bị che; cần kiểm lại liên kết object trước khi export COCO 1.0 |
| `cp4_curb`, ảnh `7d83710e-4697c3b2.jpg` | Mép cùng màu là road hay sidewalk | Ranh được xác định theo chức năng và bó vỉa, không chỉ theo màu | Chọn theo bó vỉa/chức năng; cần export lại đúng Segmentation mask 1.1 |
