#  Nhóm SmartCampus Chạy Nước Rút Trước Deadline Nộp Báo Cáo 

## ***PHẦN 1 – Thiết lập Hybrid Workflow***

***1. Sự cố mất bản Slide_BaoCao.pptx có xảy ra không nếu áp dụng Hybrid Workflow?***

- Không, sự cố mất dữ liệu vĩnh viễn sẽ không xảy ra.

- Vì sao: Nhờ mô hình Hybrid Workflow (làm việc cục bộ trên SSD kết hợp tính năng Auto Sync đẩy bản sao ngầm lên Cloud), ngay khi thành viên đó gõ lệnh lưu (Ctrl+S), file đã được đồng bộ tự động lên đám mây. Dù ổ cứng laptop hỏng vật lý, toàn bộ dữ liệu mới nhất vẫn nằm an toàn trên Cloud Server, chỉ cần đổi máy là tải về dùng tiếp được ngay.

***2. 3 bước thiết lập Auto Sync hướng dẫn cả nhóm làm ngay:***

- Bước 1: Tải và cài đặt ứng dụng đồng bộ đám mây chính hãng (như Google Drive for Desktop hoặc OneDrive).  

- Bước 2: Di chuyển toàn bộ thư mục đồ án SmartCampus vào bên trong thư mục đồng bộ của ứng dụng (hoặc chọn thư mục dự án để bật tính năng tự động đồng bộ).

- Bước 3: Kiểm tra biểu tượng trạng thái của thư mục, đảm bảo xuất hiện dấu tích xanh (Synced) hoặc thông báo Up to date, đồng thời kích hoạt tính năng Files On-Demand để tiết kiệm dung lượng ổ cứng.

***3. Thao tác khôi phục 2 trang bị xóa nhầm trong BaoCao_CuoiKy_SmartCampus.docx:***

- Không cần gõ lại từ đầu, chỉ cần mở file trên Cloud (hoặc trong thư mục đồng bộ), vào mục File $\rightarrow$ Version History (Lịch sử phiên bản) (hoặc chọn See version history).

- Tìm mốc thời gian 10 phút trước (khi chưa bị xóa nhầm) và nhấn nút Restore (Khôi phục) là xong ngay trong 1-click.

***4. Hai sai lầm về bẫy dữ liệu và cách xử lý đúng:***

- Sai lầm 1: Đồng bộ cả thư mục chứa hàng vạn file nhỏ node_modules. Điều này gây nghẽn tiến trình I/O, làm phần mềm đồng bộ bị treo (freeze) và ngốn sạch dung lượng Cloud

  - Cách xử lý đúng: Cấu hình thêm vào danh sách ngoại lệ (Exclusion Filter) để cấm tuyệt đối việc đồng bộ thư mục node_modules và .git. Khi cần, chỉ cần chạy lệnh npm install để tái tạo lại thư viện.

- Sai lầm 2: Đưa các file phim 4K nặng vào thư mục dự án để đồng bộ. Điều này làm tràn dung lượng lưu trữ Cloud không cần thiết.

   - Cách xử lý đúng: Di chuyển các file giải trí, phim ảnh ra ngoài thư mục làm việc chung của dự án.

## ***PHẦN 2 – Phân quyền chia sẻ an toàn***

Ôi mình xin lỗi nhé, bảng Markdown vừa rồi bị lỗi hiển thị các dòng phía dưới do bị dính ký tự dòng. Để mình sửa lại chuẩn xác ngay đây cho bạn copy nhé:

| File | Mức quyền | Phạm vi chia sẻ | Giải thích |
| --- | --- | --- | --- |
| **`BaoCao_CuoiKy_SmartCampus.docx`** gửi giảng viên | Viewer | Restricted (Thêm email giảng viên) | Cho phép giảng viên đọc và chấm điểm, nhưng ngăn chặn hoàn toàn việc vô tình sửa đè nội dung. |
| **`Slide_BaoCao.pptx`** cho 5 thành viên nhóm | Editor | Restricted (Chỉ thêm email 5 thành viên nhóm) | Cả nhóm cùng chung sức chỉnh sửa slide gấp trong 2 ngày mà vẫn giữ trong vòng kiểm soát nội bộ. |
| **`GopY_BanNhap.docx`** gửi anh khóa trên | Commenter | Restricted (Thêm email anh khóa trên) | Cho phép anh khóa trên để lại các dòng nhận xét, góp ý trực quan mà không làm thay đổi trực tiếp file gốc. |
| **`ThongTin_CaNhan_NhomVien.xlsx`** | Viewer (hoặc Không chia sẻ) | Restricted (Chỉ cấp nội bộ cực kỳ hạn chế) | Bảo vệ dữ liệu nhạy cảm tuyệt đối (CCCD, SĐT), không cho phép người ngoài tiếp cận dưới mọi hình thức. |

***1. Quy tắc Cờ Đỏ Bảo mật với file chứa thông tin cá nhân:***

- Quy tắc Cờ Đỏ Bảo mật: TUYỆT ĐỐI không kết hợp hai yếu tố Anyone with the link và quyền Editor đối với các tài liệu chứa dữ liệu nhạy cảm.

- Giải thích: Nếu chọn Anyone with the link, bất kỳ ai có đường dẫn trên mạng đều mở được file. Kết hợp thêm quyền Editor thì kẻ xấu hoặc bất kỳ ai lỡ tay nhấp vào link đều có thể toàn quyền sửa, xóa hoặc đánh cắp sạch thông tin cá nhân (CCCD, SĐT) của cả nhóm, dẫn đến thảm họa rò rỉ dữ liệu (Data Leak).

***2. 2 thao tác trong cửa sổ Settings để bảo vệ Slide_BaoCao.pptx:***

- Thao tác 1: Bật tính năng "Editors can't change permissions and share" (Ngăn không cho các Editor tự ý thay đổi quyền hoặc mời thêm người lạ vào file).

- Thao tác 2: Bật tùy chọn "Disable options to download, print, and copy for commenters and viewers" (hoặc giới hạn quyền tải/sao chép nếu cần thiết để bảo vệ tài sản trí tuệ của nhóm).

## ***PHẦN 3 – Tổ chức giao tiếp nhóm chuẩn doanh nghiệp***

***1. 3 vấn đề từ đoạn hội thoại Zalo hiện tại:***

- No-Hello Trap: Lan nhắn câu lửng lơ "có ai rảnh không" rồi chờ người khác đáp lại thay vì đi thẳng vào vấn đề cần giúp đỡ ngay từ đầu.

- Tin nhắn không phân luồng (Flat Stream): Tin nhắn công việc, câu hỏi trôi nổi bị trộn lẫn với ảnh meme và tán gẫu giải trí ([ảnh mèo]), khiến thông báo quan trọng rất dễ bị trôi.

- Spam @all giờ khuya: Trưởng nhóm dùng @all lúc gần 12 giờ đêm (23:47) gây phiền toái, làm gián đoạn thời gian nghỉ ngơi của các thành viên khác.

***2. Đề xuất 2 Channel/Topic trên Lark/Slack:***

- #01_thong-bao-chung: Kênh độc quyền chuyên để trưởng nhóm đăng deadline, thông báo quan trọng.

- #02_dev-code-design: Kênh chuyên trao đổi tiến độ làm slide, viết báo cáo và hỗ trợ kỹ thuật đồ án.

- #03_giai-tri-memes: Kênh riêng để gửi ảnh mèo, tán gẫu, không làm ảnh hưởng đến kênh công việc.

***3. Viết lại tin nhắn của Lan theo tư duy Asynchronous (Đủ ngữ cảnh):***

"Chào mọi người, mình đang kẹt ở phần viết sơ đồ kiến trúc hệ thống của báo cáo SmartCampus. Có bạn nào từng làm phần này rồi cho mình xin ít ý kiến hoặc rảnh qua giúp mình đoạn này với nhé. Mình để tài liệu ở link [Link Drive], mọi người xem qua giúp mình nha, cảm ơn nhiều!"

***4. Viết lại tin nhắn lúc 23:47 dùng Schedule Send và tag đích danh:***

(Sử dụng tính năng Schedule Send hẹn gửi lúc 08:00 sáng hôm sau):
"Chào @Nam và @Hoa, hệ thống ghi nhận các bạn chưa hoàn thiện phần nội dung báo cáo và slide thuyết trình. Nhờ hai bạn tập trung đẩy nhanh tiến độ và hoàn thành trước 14:00 chiều nay để kịp thời gian ghép chung với nhóm nhé!"

## ***PHẦN 4 – Thông báo tổng hợp***

[Kênh: #01_thong-bao-chung]

Chào cả nhà, mình đã tiến hành chuẩn hóa lại không gian làm việc Cloud Workspace cho đồ án SmartCampus để chuẩn bị cho hạn nộp sắp tới:

- Đã bật tính năng Auto Sync toàn bộ thư mục dự án, anh em từ nay cứ code và soạn thảo thoải mái trên máy, dữ liệu sẽ tự động được bảo vệ an toàn trên mây.

- Đã chia sẻ file báo cáo BaoCao_CuoiKy_SmartCampus.docx cho giảng viên ở chế độ Viewer / Restricted đúng chuẩn bảo mật, tránh việc sửa đè nhầm nội dung.

- @Nam và @Hoa lưu ý hoàn thiện gấp phần nội dung tiểu luận và kiểm tra lại slide báo cáo trong sáng nay nhé.