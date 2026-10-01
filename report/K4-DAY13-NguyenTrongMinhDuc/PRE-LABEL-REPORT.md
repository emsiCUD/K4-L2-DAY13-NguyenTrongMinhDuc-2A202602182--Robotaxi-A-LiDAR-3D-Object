# Báo cáo thực hành PointPillars — Day 13

## Nhóm và provenance

- Mã nhóm/phòng: làm cá nhân (LC cho miễn làm nhóm)
- Thành viên: Nguyễn Trọng Minh Đức — 2A202602182 (xem `TEAMMATES.md`)
- Trạng thái: **LC miễn bước pre-label** (ngày 2026-10-01). Không nộp output A/B/C cho LC đối chiếu.
- Ghi chú trung thực: trước khi được miễn, gói Student `student-prelabel-v1` (amd64) đã được chạy thử một lần trên laptop cá nhân, với thao tác qua trợ lý Claude Code. Kết quả: `smoke.json` báo `status: passed`. Output đã bị xóa sau khi được miễn nên **không dùng làm bằng chứng chạy thật**.

## Ba lượt inference thật

Miễn. Số liệu dưới đây chỉ ghi lại từ lần chạy thử nói trên, đọc từ `summary.csv` lúc đó. Output không còn để đối chiếu.

| Lượt | delta | Pillar XY | Số hộp | mean_z | File JSON/Side/CSV | Quan sát có bằng chứng |
| --- | --- | --- | --- | --- | --- | --- |
| A | 0 | 0.16 | 1 | 0.330 | Đã xóa | Không còn file để dẫn chứng |
| B | 1.73 | 0.16 | 13 | 1.034 | Đã xóa | Không còn file để dẫn chứng |
| C | 1.73 | 0.32 | 6 | 1.091 | Đã xóa | Không còn file để dẫn chứng |

## Ca QC có kiểm soát — không import CVAT

Miễn. Không có ca QC nào được nộp.

## Nhận xét cá nhân

- **Vai trò:** làm cá nhân; LC miễn bước tự chạy A/B/C nên em không có output để dẫn chứng. Em làm phần sửa cuboid job nguồn và QC chéo trên CVAT/portal.
- **Lỗi batch hay lỗi từng hộp:** nếu mọi hộp cùng lệch z một lượng như nhau (bằng `z_ground + delta`) thì là lỗi pipeline: phải dừng, báo LC tạo lại prediction, không sửa tay từng hộp. Nếu chỉ một vài hộp sai thì là lỗi đối tượng, kiểm từng hộp bằng nhiều góc nhìn và camera. Các lỗi em gặp trên Robotaxi thuộc loại thứ hai: hộp sai rải rác theo loại vật thể, không lệch đồng loạt.
- **Quan sát khi QC pre-label Robotaxi:**
  1. Model gán nhầm `pedestrian` khá nhiều, thường rơi vào **gốc cây**: cụm điểm đứng, hẹp, cao cỡ người nên dễ giống người trên PCD.
  2. **Bụi cây dài** ven đường hay bị nhận thành xe (`vehicles`), vì cụm điểm thấp và kéo dài giống thân xe.
  3. **Người lái xe máy** thường bị đánh hai hộp chồng lên nhau, một `two-wheels` và một `pedestrian`, cho cùng một đối tượng.
- **Cách em xử lý:** xóa hộp đặt trên cây hoặc bụi cây khi ảnh camera và các góc nhìn cho thấy không phải người hay xe. Cây không tự động thành `Obstacle`; khi không chắc em hỏi coach. Với người lái xe máy, em giữ một hộp `two-wheels` cho cả cụm và bỏ hộp `pedestrian` trùng; nếu quy ước của ca khác thì làm theo coach. Khi viết QC, em ghi rõ ID hộp, góc nhìn và ảnh camera làm bằng chứng.
- **Điều chưa chắc:** PCD Robotaxi không có intensity, nên ở vùng xa hoặc thưa em khó phân biệt thân cây với người đứng yên chỉ bằng hình dạng điểm. Những ca đó em ghi "Chưa chắc" thay vì xóa hay giữ theo model.

## LC ghi nhận riêng

- Quyền dùng PCD/image và đúng ca:
- Có chạy thật / chỉ phân tích; còn cần lượt thực hành bổ sung:
- Output đủ, giữ bản gốc, không đưa ca lỗi vào CVAT:
- Nhận xét từng thành viên và quyết định dừng pipeline:
- Đồng ý chuyển sang chỉnh/QC / cần bổ sung; lý do:
