# Xây dựng hệ thống cộng tác đám mây cho doanh nghiệp

## ***PHẦN 1: Thiết kế Kiến trúc (Cấu trúc thư mục Cloud & Quy trình phân quyền)***

***1. Sơ đồ cấu trúc thư mục Cloud an toàn***

Để 10 thành viên phân bổ tại Hà Nội, Đà Nẵng và TP.HCM làm việc mượt mà không dẫm chân lên nhau, không gian lưu trữ chung (Smart-Home Workspace) được phân cấp thành các tầng thư mục chuẩn chỉnh dựa trên sơ đồ thiết kế:

📁 Smart-Home Workspace/
├── 📁 00_Company/                      (Tài liệu chung công ty)
│   ├── 📁 00_Giới thiệu công ty
│   ├── 📁 01_Nội quy & Quy trình
│   └── 📁 02_Biểu mẫu & Tài liệu chung
│
├── 📁 01_Operation/                    (Phòng Vận hành)
│   ├── 📁 01_Kế hoạch & Báo cáo
│   ├── 📁 02_Quy trình vận hành
│   └── 📁 03_Tài liệu hướng dẫn
│
├── 📁 02_Project/                      (Khối Dự án chuyên môn)[cite: 5]
│   ├── 📁 Dự án A - Nhà thông minh[cite: 5]
│   ├── 📁 Dự án B - IoT Gateway[cite: 5]
│   └── 📁 Dự án C - Ứng dụng Mobile[cite: 5]
│
├── 📁 03_Marketing/                    (Khối Marketing & Truyền thông)[cite: 5]
│   ├── 📁 01_Kế hoạch Marketing[cite: 5]
│   ├── 📁 02_Nội dung & Hình ảnh[cite: 5]
│   └── 📁 03_Báo cáo & Phân tích[cite: 5]
│
├── 📁 04_Finance/                      (Khối Tài chính - Kế toán - Nhạy cảm)[cite: 5]
│   ├── 📁 01_Kế toán - Thu chi[cite: 5]
│   ├── 📁 02_Hợp đồng & Hóa đơn[cite: 5]
│   └── 📁 03_Báo cáo tài chính[cite: 5]
│
└── 📁 99_Archive/                      (Thùng rác / Kho lưu trữ dữ liệu cũ)[cite: 5]

***2. Quy trình phân quyền theo phòng ban và Cơ chế Chuyển giao (Transfer Ownership)***

Dựa theo ma trận phân quyền tối thiểu (Principle of Least Privilege), mỗi phòng ban chỉ có quyền thao tác trên đúng không gian của mình để tránh rò rỉ dữ liệu[cite: 5]:

***Bảng phân quyền chi tiết:***

- 00_Company: Admin có Toàn quyền, các phòng ban khác và Thành viên có quyền Xem[cite: 5].

- 01_Operation: Bộ phận Operation có quyền Chỉnh sửa, các khối khác chỉ Xem[cite: 5].

- 02_Project: Nhóm Project có quyền Chỉnh sửa, các khối khác chỉ Xem[cite: 5].

- 04_Finance: Nhóm Finance quản lý Chỉnh sửa, tuyệt đối bảo mật với các thành viên ngoài luồng tài chính[cite: 5].

- Cơ chế Transfer Ownership (Chuyển giao quyền chủ sở hữu): Khi một trưởng phòng hoặc Admin rời nhóm, trước khi xóa tài khoản, họ bắt buộc phải thực hiện lệnh Transfer Ownership thư mục gốc/file cho người kế nhiệm. Ngoài ra, thiết lập tài khoản Cloud ở dạng Enterprise Shared Drive để quyền sở hữu thuộc về tổ chức (Organization) thay vì phụ thuộc vào một cá nhân đơn lẻ, triệt tiêu rủi ro mất quyền quản trị khi nhân sự biến động. 

## ***PHẦN 2: Bản quy ước văn hóa & Xử lý tình huống (Edge Case)***

***1. Bản quy ước văn hóa bất đồng bộ (Asynchronous Culture) để không ai bị làm phiền đêm muộn***

Vì team làm việc từ xa dễ dẫn đến việc nhắn tin vô tội vạ bất kể thời gian, startup áp dụng ngay 3 quy tắc vàng:

- Ứng dụng tính năng Schedule Send (Hẹn giờ gửi tin nhắn): Nếu nảy ra ý tưởng hoặc công việc vào lúc 23:00 đêm, tuyệt đối không bấm Send trực tiếp gây thông báo quấy rầy đồng nghiệp. Hãy dùng tính năng Schedule Send để hẹn lịch gửi tự động vào 09:00 sáng hôm sau.

- Quy tắc chống No-Hello Trap và cấm Spam @all: Không bao giờ nhắn một câu trống không kiểu "Ê mọi người ơi" rồi để đấy. Phải gom nội dung thành một message đầy đủ context (Vấn đề gì? Cần ai xử lý? Deadline khi nào?). Nghiêm cấm dùng @all hoặc @channel sau 21:00 tối trừ khi hệ thống server sập (khẩn cấp)[cite: 5].

- Tôn trọng khung giờ tập trung và Trạng thái "Không làm phiền" (Do Not Disturb): Khung giờ làm việc chung là 09:00 - 18:00 (T2 - T6)[cite: 5]. Ngoài giờ này, mọi người có quyền tắt thông báo công việc để tái tạo năng lượng, đảm bảo sức khỏe tinh thần cho toàn bộ 10 thành viên ở 3 miền[cite: 5].

***2. Giải quyết Edge Case: "Lỡ tay xóa sạch thư mục dự án trên Cloud thì cứu bằng cách nào?"***

Nếu một thành viên vô tình quét chọn và xóa sạch thư mục dự án trên Cloud Workspace, kiến trúc hệ thống của chúng ta có các lớp "phao cứu sinh" sau để phục hồi nguyên trạng lập tức:

- Lớp 1: Thùng rác hệ thống Cloud (Cloud Trash / Bin): Toàn bộ file/thư mục bị xóa không biến mất vĩnh viễn ngay lập tức mà được chuyển vào thư mục Trash của Workspace (hoặc 99_Archive). Chỉ cần Admin hoặc người có quyền truy cập vào Thùng rác, nhấn chuột phải chọn Restore, toàn bộ cấu trúc thư mục và file sẽ quay lại đúng vị trí ban đầu trong 1 nốt nhạc.

- Lớp 2: Tính năng Version History / Activity Log: Các nền tảng Cloud doanh nghiệp đều lưu lịch sử nhật ký hoạt động (Audit Log). Nếu ai đó lỡ sửa đè hoặc xóa nhầm cấu trúc file sâu bên trong, hệ thống cho phép quay ngược thời gian (Rollback) về thời điểm trước khi xảy ra thảm họa.

- Lớp 3: Bản backup định kỳ cục bộ (Hybrid Backup): Nhờ mô hình Hybrid Workflow (đã thiết lập ở các bài trước), các lập trình viên luôn có các bản source code đồng bộ cục bộ trên máy cá nhân hoặc trên các nhánh Git dự phòng. Do đó, hệ thống miễn nhiễm với các tai nạn thao tác nhầm trên mây.