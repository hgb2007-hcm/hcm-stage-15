# Tự Thiết Kế Hệ Thống Lưu Trữ và Phân Quyền Cho Dự Án CampusPay

## ***PHẦN 1: Phân tích bài toán (I/O)***

***1. Xác định Đầu vào và Đầu ra***

***Input***

- Cấu trúc nhân sự gồm 4 ban nhóm / vai trò: Ban Kỹ thuật (3 người), Ban Thiết kế (2 người), Ban Kinh doanh (2 người), Nhà đầu tư bên ngoài (1 người).

- Các loại tài liệu dự án đa dạng: Mã nguồn/API, Tài liệu thiết kế (UI/UX), Kế hoạch kinh doanh (Business Plan), Slide Pitch, Dữ liệu khách hàng nhạy cảm.

- Các ràng buộc bảo mật: Nguyên tắc tối thiểu quyền, chống rò rỉ dữ liệu, giới hạn thời gian truy cập.

***Output***

- Cấu trúc thư mục Google Drive 2 cấp phân tách rõ ràng.

- Bảng phân quyền chi tiết cho từng đối tượng theo đúng nguyên tắc bảo mật.

- Quy trình thiết lập hệ thống từ lúc khởi tạo đến khi thu hồi quyền.   

***2. Đề xuất cấu trúc thư mục Google Drive 2 cấp***

Tổ chức không gian lưu trữ chung thành các thư mục phân cấp rõ ràng để dễ quản lý:

📁 CampusPay_Master_Workspace/ (Thư mục gốc)
├── 📁 01_Ban_Ky_Thuat/         (Chứa source code, tài liệu API, database test)
├── 📁 02_Ban_Thiet_Ke/         (Chứa file thiết kế UI, assets đồ họa, Figma links)
├── 📁 03_Ban_Kinh_Doanh/       (Chứa Business Plan, tài liệu tài chính, data khách hàng)
├── 📁 04_Chung_Du_An/          (Chứa Slide Pitch, tài liệu giới thiệu chung dùng chung)
└── 📁 99_Archive_Trash/        (Thùng rác / Lưu trữ dữ liệu cũ)

***3. Các bước thiết lập hệ thống tuần tự***

- Bước 1 — Khởi tạo cấu trúc: Tạo thư mục gốc CampusPay_Master_Workspace và các thư mục cấp 2 tương ứng cho từng ban như thiết kế trên.

- Bước 2 — Đặt tên quy chuẩn: Đặt tên file/thư mục có, rõ ràng, không dùng các từ ngữ cảm tính (tránh dùng v_final_cuối_cùng).

- Bước 3 — Cấp quyền theo nhóm (Group-based Permission): Thiết lập quyền truy cập ở mức thư mục (Folder level) dựa trên nguyên tắc tối thiểu quyền (ví dụ: Ban Kỹ thuật có quyền vào thư mục 01_Ban_Ky_Thuat, các ban khác để chế độ Restricted hoặc ẩn).

- Bước 4 — Kiểm tra và Test hệ thống: Dùng tài khoản phụ (hoặc nhờ thành viên khác test thử) để kiểm tra xem phân quyền đã chặn đúng các thư mục nhạy cảm chưa.

- Bước 5 — Thu hồi và Cập nhật định kỳ: Thiết lập ngày hết hạn (Expiration Date) cho các tài khoản ngoài (như Nhà đầu tư) và thu hồi quyền ngay lập tức khi nhân sự thay đổi.

## ***PHẦN 2: Triển khai & Chống lỗi***

***Bẫy 1 (Kinh doanh muốn chia sẻ Business Plan cho Kỹ thuật xem nhưng không cho sửa):***

 - Giải pháp: Cấp quyền Viewer (hoặc Commenter) cho các thành viên Ban Kỹ thuật đối với riêng file Business Plan đó.

 - Lý do: Quyền Viewer chỉ cho phép đọc nội dung để hiểu ngữ cảnh làm sản phẩm (context) mà hoàn toàn không thể chỉnh sửa hay làm xáo trộn dữ liệu gốc, tuân thủ tuyệt đối nguyên tắc tối thiểu quyền.

 ***Bẫy 2 (Nhà đầu tư muốn tải PDF Slide Pitch về xem offline nhưng không được sửa file gốc):***

 - Giải pháp: Cấp quyền Viewer và KHÔNG tắt tính năng "Download/Print/Copy for commenters and viewers" (vẫn giữ mặc định cho phép tải).

 - Lý do: Vì nhà đầu tư cần file PDF để xem offline, việc cho phép họ tải về bản PDF đọc độc lập không ảnh hưởng gì tới file gốc trên Google Drive (vốn là file định dạng Google Slides/PowerPoint trực tuyến không bị tác động chỉnh sửa). Trừ khi đó là tài liệu tối mật tuyệt đối mới cần cân nhắc tắt tính năng tải xuống.

 ***Bẫy 3 (Thành viên nghỉ việc — cần khóa truy cập tức thì):***

 - Thao tác cần làm ngay lập tức:

   - Vào bảng quản trị tài khoản tổ chức (Google Workspace Admin Console) hoặc danh sách Share của thư mục gốc, Xóa (Remove) thẳng email cá nhân của nhân sự đó khỏi danh sách cấp phép.

   - Đổi toàn bộ các đường link chia sẻ nội bộ sang chế độ Restricted.

   - Kiểm tra xem nhân sự đó có từng tải dữ liệu nhạy cảm ra thiết bị cá nhân trước đó không (nếu cần thiết thực hiện thu hồi thiết bị vật lý). Thao tác này cắt đứt hoàn toàn quyền truy cập Cloud ngay lập tức.

