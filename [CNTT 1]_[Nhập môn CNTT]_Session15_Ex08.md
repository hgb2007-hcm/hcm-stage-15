# Kiểm Tra Vì Sao Dữ Liệu Khách Hàng CampusPay Bị Rò Rỉ Ra Ngoài

## ***PHẦN 1: Phân tích lỗ hổng***

***1. Xác định nguồn gốc trực tiếp gây rò rỉ dữ liệu***

- Giải thích kỹ thuật:
  - Tổ hợp quyền + phạm vi lỗi: Thành viên A đã kết hợp giữa Editor (Toàn quyền chỉnh sửa, thêm, xóa, đổi tên và chia sẻ) và chế độ phạm vi Anyone with the link (Bất kỳ ai trên Internet có đường dẫn đều mở được mà không cần đăng nhập).

  - Cơ chế rò rỉ: Khi một file chứa danh sách thông tin nhạy cảm (họ tên, SĐT, số dư tài khoản) bị mở toang cửa bằng link công khai, bất kỳ ai lướt qua đường dẫn đều có thể xem, tải xuống hoặc thậm chí vô tình/cố ý ném link đó lên các nhóm Facebook công khai. Kết quả là dữ liệu khách hàng bị phát tán ra ngoài tầm kiểm soát hoàn toàn.

***2. Ba giả thuyết về việc "Email người lạ" truy cập được campuspay_api_keys.txt***

Mặc dù file campuspay_api_keys.txt lúc 09:30 T2 được cấu hình chế độ Restricted cho email B, nhưng kẻ lạ vẫn đột nhập được vào lúc 10:00 T3 do các nguyên nhân sau:

- Giả thuyết 1 (Lộ tài khoản cá nhân): Email B hoặc Trưởng nhóm đã bị lộ mật khẩu, dính keylogger hoặc để lộ phiên đăng nhập, tạo cơ hội cho kẻ lạ dùng chính tài khoản được cấp phép để truy cập vào file.

- Giả thuyết 2 (Bị thay đổi quyền chia sẻ trung gian): Sau khi được cấp quyền ở dạng Restricted, một người có quyền quản trị/chỉnh sửa đã vô tình hoặc chủ ý đổi phạm vi chia sẻ thành Anyone with the link trong một khoảnh khắc nào đó, khiến link bị phát tán rồi sau đó mới đổi lại trạng thái Restricted.

- Giả thuyết 3 (Truy cập thông qua liên kết dịch vụ bên thứ ba / Chia sẻ file dạng Workspace trỏ nhầm): File này được lưu trong một thư mục chung (Shared Folder), và kẻ lạ có quyền truy cập vào thư mục cha đó, dẫn đến việc kế thừa quyền mở các file bên trong mặc dù file lẻ được đặt chế độ Restricted.

## ***PHẦN 2: Sửa lỗi & Nghiệm thu***

***1. Bảng cấu hình quyền chia sẻ đúng chuẩn***

***campuspay_beta_users.xlsx***

- Viewer (hoặc ẩn hoàn toàn nếu không cần thiết)

- Restricted (Chỉ thêm đích danh email những người thực sự cần test)

- Ngăn chặn tuyệt đối việc rò rỉ thông tin cá nhân và số dư tài khoản sinh viên ra cộng đồng.

***campuspay_api_keys.txt***

- Viewer
- Restricted (Chỉ cấp cho Backend Developer chủ chốt)
- Thông tin cấu hình kỹ thuật cốt lõi, tuyệt đối không cho phép người ngoài hoặc bên thứ ba chỉnh sửa hoặc đọc tràn lan.

***slide_pitch_v8.pptx***

- Commenter hoặc Editor (tùy nhu cầu làm việc nhóm)

- Restricted (Chia sẻ nội bộ nhóm startup CampusPay)

- Để các thành viên trong nhóm cùng góp ý, chỉnh sửa slide dễ dàng mà không sợ bị lọt thông tin chiến lược ra ngoài trước ngày pitching.

***2. Vô hiệu hóa link cũ và tính năng "Set Expiration Date"***

- Tính năng vô hiệu hóa link cũ nhanh nhất trong Google Drive:
Vào phần Share (Chia sẻ) của file/thư mục $\rightarrow$ Chuyển phần quyền truy cập chung (General access) từ Anyone with the link về lại Restricted (Hạn chế). Ngay lập tức, mọi đường link cũ đang bị trôi nổi trên mạng xã hội sẽ trở thành "link chết", bất kỳ ai bấm vào đều nhận được thông báo lỗi không có quyền truy cập.

- Khi nào nên dùng tính năng "Set Expiration Date" (Đặt ngày hết hạn)?

  - Nên dùng tính năng này khi chia sẻ tài liệu, bản build, hoặc mã nguồn mang tính tạm thời cho cộng tác viên bên ngoài, Mentor, giám khảo hoặc đối tác trong một khoảng thời gian giới hạn (ví dụ: chỉ cho phép xem trong 3 ngày diễn ra sự kiện pitching).

  - Sau thời gian đó, quyền truy cập của họ sẽ tự động bị thu hồi mà không cần lập trình viên phải nhớ vào bấm thủ công, giúp siết chặt vòng đời bảo mật dữ liệu.