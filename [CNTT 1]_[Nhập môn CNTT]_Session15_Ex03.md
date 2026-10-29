# Ngăn chặn quá tải lưu trữ khi đồng bộ mã nguồn

## **PHẦN 1: Báo cáo phân tích I/O (Luồng dữ liệu từ lúc nhấn Ctrl+S đến tích xanh)**

Khi đang code hăng say mà bấm Ctrl+S trong Visual Studio Code, luồng xử lý I/O (Input/Output) giữa máy tính và đám mây sẽ diễn ra qua các bước cụ thể sau:


**1. Ghi xuống ổ cứng cục bộ (Local Write):**
Ngay khi nhấn Ctrl+S, hệ điều hành sẽ nhận lệnh và ghi đè nội dung mới từ RAM xuống ổ cứng SSD cục bộ (D:\project\src\...). Lúc này, file trên máy tính đã được cập nhật phiên bản mới nhất và trình soạn thảo hết dấu chấm tròn (saved).

**2. Phát hiện sự thay đổi (File System Watcher):**
Tiến trình ngầm của ứng dụng Cloud Sync (như OneDrive, Google Drive) liên tục theo dõi sự thay đổi của thư mục dự án. Nó phát hiện ra file vừa có sự thay đổi thời gian chỉnh sửa (timestamp).

**3. Kiểm tra bộ lọc ngoại lệ (Exclusion Filter Check):**
Hệ thống kiểm tra xem file này có nằm trong danh sách bị loại trừ hay không (ví dụ kiểm tra có thuộc thư mục node_modules hay .git không). Do file này nằm trong thư mục src, nó vượt qua bộ lọc an toàn và được phép đưa vào hàng đợi đồng bộ.

**4.Đóng gói và Tải lên Cloud (Network Upload):**
Ứng dụng bắt đầu băm nhỏ và truyền tải gói dữ liệu thay đổi qua Internet lên máy chủ đám mây (Cloud Server).

**5.Xác nhận hoàn tất (Sync Complete):**
Khi server phản hồi đã nhận đủ dữ liệu, ứng dụng đồng bộ trên máy tính sẽ đổi biểu tượng file từ vòng xoay đang tải sang dấu tích xanh thân thuộc, báo hiệu dữ liệu đã được bảo vệ an toàn trên mây.

## **PHẦN 2: Thiết kế cấu trúc thư mục Cloud & Xử lý bẫy node_modules**

Để không bao giờ gặp phải cảnh phần mềm đồng bộ bị treo cứng (freeze) hay tràn dung lượng ổ cứng Cloud vì gánh hàng vạn file rác, mình thiết kế lại mô hình Hybrid Workflow chuẩn chỉnh như sau:

***1. Cách xử lý bẫy node_modules***

- ***Nguyên nhân gây sập hệ thống:*** Thư mục node_modules chứa hàng chục ngàn file nhỏ xíu của các gói thư viện (như react, lodash, axios...). Khi bật Auto Sync, phần mềm phải quét và đồng bộ từng file một, gây nghẽn tiến trình I/O làm treo máy (freeze) và ngốn sạch hạn mức lưu trữ Cloud.   

- ***Giải pháp khắc phục:***

  - Cấu hình tính năng Exclusion (Loại trừ) trong phần mềm Cloud để cấm tuyệt đối việc đồng bộ thư mục node_modules và .git.

  - Nguyên tắc vàng: Source code tự viết (src, package.json) thì đẩy lên Cloud. Còn thư mục node_modules thì không bao giờ đồng bộ. Khi chuyển máy hoặc tải về dự án mới, chỉ cần mở terminal chạy lệnh npm install, hệ thống sẽ tự động tải lại toàn bộ thư viện chuẩn chỉnh trong vòng chưa tới 1 phút.

***2. Sơ đồ tổ chức thư mục Cloud chuẩn cho Lập trình viên***

📁 Cloud_Workspace/ (Đồng bộ Hybrid + Files On-Demand)
│
├── 📁 01_Source_Code/                 <-- Khu vực chứa mã nguồn dự án
│   └── 📁 smart_food_app/
│       ├── 📁 src/                    (Code chính: JS, CSS, Components - ĐƯỢC SYNC)
│       ├── 📁 public/                 (Assets tĩnh cho web - ĐƯỢC SYNC)
│       ├── 📄 package.json            (Bản đồ khai báo thư viện - ĐƯỢC SYNC)
│       └── 🚫 node_modules/           (ĐÃ BỊ LOẠI TRỪ KHỎI SYNC - Bẫy dữ liệu)
│
├── 📁 02_Documentation/               <-- Tài liệu dự án
│   ├── 📄 SRS_System_Requirement.docx (Tài liệu đặc tả yêu cầu hệ thống)
│   └── 📄 Database_Design.png         (Sơ đồ thiết kế cơ sở dữ liệu)
│
└── 📁 03_Assets_Media/                <-- Tài nguyên thiết kế & Marketing
    ├── 📁 logo_brand/                 (Logo Startup Smart Food từ Canva)
    └── 📁 video_demo/                 (Video quay màn hình demo sản phẩm)


***3. Kích hoạt tính năng "Files On-Demand"***

- Bật chế độ Files On-Demand (Tệp theo yêu cầu) trên OneDrive/Google Drive.

- Tính năng này giúp các file trên Cloud chỉ hiển thị "hình bóng" (dung lượng chiếm trên ổ cứng Local bằng 0 KB) nhưng vẫn bấm vào xem được. Khi nào lập trình viên mở file làm việc, máy mới tự tải về, giúp tiết kiệm tối đa dung lượng SSD cho chiếc Laptop học tập.