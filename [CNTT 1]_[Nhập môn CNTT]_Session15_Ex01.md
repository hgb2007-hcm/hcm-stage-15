#  Loại bỏ rủi ro lưu trữ cục bộ trong dự án nhóm

## ***PHẦN 1: Phân tích hậu quả "Thảm họa dữ liệu truyền thống" của Nam***

Nhìn cảnh ông Nam ôm đầu ngồi trước máy tính lúc gần 12 giờ đêm mà thấy thương nhưng cũng... đúng là "gậy ông đập lưng ông". Việc lưu trữ dữ liệu kiểu truyền thống (lưu local trên ổ cứng rồi ném file qua Zalo) đang đẩy Nam vào những hậu quả cực kỳ cay đắng:

- Mất sạch dữ liệu vĩnh viễn (Ghi đè nhầm + Ctrl + S):
Do quá mệt mỏi, Nam lỡ tay xóa mất đoạn code xử lý đơn hàng quan trọng rồi lại nhấn phản xạ Ctrl + S. Trên ổ cứng máy tính cá nhân, một khi đã lưu đè kiểu này mà không bật tính năng tự động backup hay version history thì coi như "bay màu" vĩnh viễn, không có nút Undo thần kỳ nào cứu nổi.

- Rối tung rối mù vì quản lý phiên bản thủ công bằng tên file (v1, v2_final):
Cách đặt tên kiểu main_v1.py, main_v2.py, rồi chốt hạ bằng main_v2_final.py gửi qua Zalo là ác mộng thực sự. Khi nhóm giục dồn dập (Linh: Code final rồi gửi nhé Nam!, Huy: Mai demo với khách đó ợ), rất dễ gửi nhầm bản cũ, dẫn đến việc team mỗi đứa một phách, không kiểm soát được đâu là dòng code chuẩn mới nhất.

- Độc lập cục bộ thành "tử huyệt" (Single Point of Failure):
Câu cửa miệng "File chỉ có trên máy em" chính là quả bom nổ chậm lớn nhất. Máy tính cá nhân có thể lăn đùng ra hỏng ổ cứng, sập nguồn, dính virus bất cứ lúc nào. Không có bản dự phòng trên Cloud hay Git, Nam đang đánh cược toàn bộ công sức của cả đội Startup "Smart Food" vào một chiếc laptop duy nhất.

## ***PHẦN 2: Đề xuất quy trình "Hybrid Workflow" (Kết hợp Local & Cloud/Git) cho Nam***

**1. Sử dụng Git & GitHub/GitLab (Version Control chuyên nghiệp):**

- Local Git: Thay vì lưu chục cái file rác rưởi _v1, _final, Nam chỉ cần giữ đúng một file main.py và thực hiện lệnh git commit mỗi khi hoàn thành một tính năng nhỏ. Mỗi commit giống như một mốc lịch sử (checkpoint game), thích quay lại bản nào chỉ mất đúng 1 câu lệnh hoặc 1 cú click chuột.

- Remote Repository (GitHub/GitLab): Đẩy code lên kho lưu trữ trên mây sau mỗi buổi code. Dù máy tính có hỏng hay mất, code vẫn nằm an toàn trên GitHub.

**2. Kích hoạt tính năng Cloud Version History (OneDrive/Google Drive):**

- Nếu lười dùng Git dòng lệnh, Nam có thể lưu thư mục project trực tiếp trong OneDrive hoặc Google Drive được đồng bộ realtime. Các dịch vụ này đều có tính năng Version History (lịch sử phiên bản).

- Nếu lỡ tay xóa code và lưu đè, Nam chỉ cần mở lịch sử phiên bản trên web cloud, chọn mốc thời gian trước đó và nhấn khôi phục. Chỉ 1-click là xong, không phải khóc thét.

**3. Quy trình làm việc nhóm chuẩn Startup:**

- Thay vì ném file qua Zalo, nhóm "Smart Food" nên tạo chung một Repository trên GitHub. Các thành viên code trên nhánh riêng (branch) rồi tạo Pull Request để merge code, vừa sạch sẽ, chuyên nghiệp lại không bao giờ sợ xung đột phiên bản.