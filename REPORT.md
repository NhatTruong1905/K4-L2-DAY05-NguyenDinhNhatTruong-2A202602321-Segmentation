# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602321
- Ngày / CVAT local: 17/09/2026 / http://localhost:8080
- Công cụ đã dùng: Brush, Polygon, Intelligent Scissors, CVAT Model Integrator (gợi ý tự động)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Tất cả 9 task đều đã Save và export thành công đầy đủ các file ZIP đúng định dạng quy định (`Segmentation mask 1.1` cho semantic, `COCO 1.0` cho instance/panoptic) vào thư mục `submissions/`, kiểm tra qua script QC `inspect_submissions.py` đạt trạng thái `[OK]`.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg`, chiếc xe bán tải màu xám bạc ở làn giữa phía sau người phụ nữ áo dài (khu vực trung tâm ảnh, bbox khoảng x=271, y=115).
- Class và quy tắc tôi dùng để chọn biên: Class `car`. Quy tắc chọn biên: Chỉ vẽ phần thân vỏ và bánh xe thực sự nhìn thấy trên ảnh; dừng mask tại ranh giới bị che khuất bởi vạt áo dài của người đi bộ phía trước và người đi xe máy bên phải, tuyệt đối không tự suy đoán đường viền vẽ xuyên qua thân người phía trước.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Vùng gợi ý ban đầu bị tràn viền qua phần vạt áo dài trắng của người đi bộ và lem vào bóng râm/mặt đường dưới gầm xe; tôi đã dùng công cụ Brush/Eraser để xóa phần lem sang áo dài và tỉa lại viền cong của lốp xe đúng ranh giới tiếp xúc với mặt đường.
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `easy_semantic`, ảnh `817bca71-00000000.jpg` và `81ae7cbb-6bc63a4a.jpg`, khu vực nhà ở hai bên đường và thảm cây cỏ ven đường.
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: thiếu-thừa vật / bỏ sót lớp (thiếu hoàn toàn lớp `building` và thiếu diện tích `vegetation`).
- Bằng chứng tôi nhìn thấy: Khi rà soát lại bài, nhận thấy trong 2 ảnh khu dân cư có các dãy nhà ở rõ ràng nhưng chưa có pixel nào được gán cho nhãn `building`, diện tích cây cối (`vegetation`) mới chỉ vẽ vài vệt nhỏ, bỏ sót nhiều thảm cỏ và tán cây ven đường.
- Quy tắc và hành động sửa: Mở lại task `easy_semantic` trên CVAT, dùng Polygon/Brush tô phủ kín các khối nhà dân cho class `building` và tô bổ sung các thảm cỏ, lùm cây xanh cho class `vegetation`.
- Sau sửa đã Save và export lại chưa? Đã Save trên CVAT và export lại file `easy_semantic.zip` chuẩn định dạng `Segmentation mask 1.1` đè vào thư mục `submissions/`.

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): Sau khi sửa và export lại, ảnh `817bca71-00000000` đã bổ sung 145.643 pixels và ảnh `81ae7cbb-6bc63a4a` bổ sung 73.207 pixels cho `building`, diện tích `vegetation` tăng lên rõ rệt; script `inspect_submissions.py` kiểm tra đạt chuẩn `[OK]`. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 1. `cp4_curb`, ảnh `7d83710e-4697c3b2.jpg`, dải đất và túi rác nằm giữa gờ bó vỉa và lối đi bê tông | Gán là `sidewalk` hay `road` / `background` | Quy tắc `cp4_curb`: Ranh giới road vs sidewalk là ranh giới chức năng dựa trên gờ bó vỉa (curb), không phụ thuộc vào màu sắc hay vật liệu mặt đường; toàn bộ phần sau mép bó vỉa không dành cho xe lưu thông | Quyết định gán dải đất sau bó vỉa vào `sidewalk`. Xin coach xác nhận: dải đất/hố cây nằm sau gờ bó vỉa nên gom chung vào `sidewalk` hay để `background`? |
| 2. `hard_panoptic`, ảnh `000000460147.jpg`, các xe ô tô con nằm trên rơ-moóc xe tải chở ô tô chuyên dụng | Gom chung toàn bộ vào một instance `truck`, hay tách từng xe con thành instance `car` riêng nằm trên `truck` | Các xe con có hình dáng và đặc điểm nhận diện hoàn chỉnh của `car`, nhưng đồng thời đang là hàng hóa trên xe chuyên dụng | Quyết định tách riêng từng xe con thành instance `car` độc lập để bảo toàn thông tin đối tượng. Nhờ coach giải đáp quy ước chuẩn của lớp đối với xe trên xe chuyên dụng |
| 3. `cp3_thin`, ảnh `839f7736-abe28069.jpg`, hệ giàn khung thép kim loại đỡ biển báo cao tốc bắc ngang đường | Gán toàn bộ giàn khung thép và biển báo là `traffic sign`, hay tách giàn thép thành `pole` và mặt bảng chữ là `traffic sign` | Khung thép giàn có kết cấu thanh chịu lực kim loại (tương tự `pole`), còn bảng chữ hiển thị thông tin chỉ đường là `traffic sign` | Quyết định dùng brush 2px tách hệ khung giàn thép vào `pole` và mặt bảng chữ vào `traffic sign`. Xin coach góp ý độ chi tiết của brush ở khoảng cách xa như vậy đã đạt yêu cầu chưa |
