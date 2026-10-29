# Giải Cứu Dữ Liệu Nhóm Startup Bị Mất Trên Máy Tính Cá Nhân

## ***PHẦN 1: Phân tích vấn đề***

***1. Phân tích rủi ro các hạng mục tài liệu của nhóm CampusPay***

- Business Plan (20 trang)
  - 100% (Mất trắng)

  - Công nghệ & Tổ chức: Phụ thuộc vào một thiết bị đơn lẻ (Single Point of Failure), không có cơ chế tự động sao lưu lên mây (Cloud), toàn bộ sức lao động 3 tháng nằm trọn trên chiếc ổ cứng vật lý dễ hỏng.

- Mã nguồn giao diện (HTML/CSS)

  - 90% (Dễ hỏng/Thất lạc)

  - Công nghệ: Dùng USB di động làm kho lưu trữ chính. USB rất dễ bị hỏng chip nhớ, thất lạc, nhiễm virus hoặc không có tính năng kiểm soát phiên bản.

- Database danh sách test user

  - 80% (Lệch pha dữ liệu)

  - Tổ chức: Lưu trữ cục bộ phân tán, dùng phiên bản cũ từ 2 tuần trước do không có quy trình cập nhật và đồng bộ dữ liệu tập trung giữa các thành viên.

- Slide Pitch (PowerPoint)

  - 50% (Nhầm lẫn phiên bản)

  - Tổ chức: Đặt tên file thủ công hỗn loạn (version hell) qua chat Zalo cá nhân, không kiểm soát được đâu là bản hoàn thiện chính thức cuối cùng trước khi thuyết trình.

- Tài liệu SRS (Word)

  - 70% (Trôi thông tin, thất lạc)

  - Tổ chức: Trao đổi qua email đính kèm rời rạc, không có nguồn chân lý duy nhất (Single Source of Truth), dẫn đến việc mỗi người sửa một phách, không biết bản nào là chuẩn.

***2. Hiện tượng "23 phiên bản file tồn tại trên nhiều thiết bị"***

- Tên gọi kỹ thuật: Hiện tượng này được gọi là Version Hell (Địa ngục phiên bản) hoặc Fragmented Version Control (Quản lý phiên bản phân mảnh thủ công).

- Nguyên nhân gây ra: Do nhóm thiếu một không gian làm việc chung (Cloud Workspace hoặc Version Control system như Git). Thay vào đó, nhóm chọn cách gửi file qua lại bằng Zalo, mỗi lần tải về sửa lại đổi tên thành _v1, _v2, _final, _final_FINAL... khiến file bị nhân bản vô tội vạ trên máy của từng người.

- Hậu quả đối với dự án thực tế:

   - Dẫn đến việc dùng nhầm mã nguồn cũ, mất sạch các tính năng mới đã viết.
   - Xung đột logic nghiêm trọng khi ghép code của các thành viên lại với nhau trước ngày demo.

   - Tốn rất nhiều thời gian vô ích chỉ để ngồi dò xem file nào mới thực sự là file chạy được, gây áp lực tâm lý cực lớn cho toàn đội.  '

## ***PHẦN 2: Sửa lỗi & Nghiệm thu***  

**1. Thiết kế quy trình Hybrid Workflow cho nhóm CampusPay***

Để không bao giờ lặp lại thảm họa mất dữ liệu, nhóm cần thiết lập ngay mô hình làm việc kết hợp (Hybrid Workflow):

- Sơ đồ luồng dữ liệu 3 bước:

 [Máy tính Local (SSD)] 
       │ (Nhấn Ctrl+S)
       ▼
[Phần mềm Auto Sync (OneDrive / Google Drive)] 
       │ (Đồng bộ ngầm tự động qua Internet)
       ▼
[Cloud Storage (Đám mây an toàn)]

- Phần mềm sử dụng: Google Drive Desktop hoặc OneDrive tích hợp sẵn tính năng Files On-Demand.

- Thư mục được Sync: Toàn bộ thư mục chung của dự án (D:/CampusPay/) bao gồm thư mục chứa source code chính, tài liệu SRS, Business Plan và Slide.

- Thư mục KHÔNG nên Sync: Các thư mục rác, file cài đặt nặng (.exe, .iso, .zip dung lượng lớn), thư mục game, phim cá nhân hoặc các file cache tạm thời của hệ thống để tránh làm đầy dung lượng mây và gây nghẽn băng thông.

***2. Xử lý "Bẫy dữ liệu" khi đồng bộ Cloud***

- Biểu tượng trạng thái trên File Explorer: Khi thành viên gập máy ngay khi vừa sửa file mà chưa kịp sync xong, biểu tượng bên cạnh file sẽ hiển thị vòng tròn xoay (đang đồng bộ / pending) hoặc dấu chấm than màu vàng (báo lỗi đồng bộ do mất kết nối mạng) thay vì dấu tích xanh (Synced).

- Hệ thống xử lý thế nào: Quá trình tải lên dữ liệu bị ngắt quãng đột ngột. Trên mây (Cloud) vẫn lưu giữ phiên bản cũ trước đó, còn bản sửa đổi mới nhất chỉ nằm cục bộ trên chiếc laptop vừa bị gập lại. Khi mở máy ở thiết bị khác, dữ liệu mới sẽ "bốc hơi" hoặc gây xung đột phiên bản (Sync Conflict). 

- Cách khắc phục cho thành viên:

  - Trước khi gập máy hoặc rời khỏi bàn làm việc, hãy liếc mắt xuống System Tray (góc dưới bên phải màn hình Windows) để kiểm tra icon OneDrive/Google Drive xem đã hiện thông báo "Up to date" (Đã cập nhật) chưa.
  - Tập thói quen đợi biểu tượng tích xanh xuất hiện trên thư mục dự án rồi mới được phép tắt máy hoặc ngắt kết nối Wi-Fi.