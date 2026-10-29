# Ngăn chặn rò rỉ dữ liệu do chia sẻ sai quyền

## **PHẦN 1: Phân tích nguyên nhân hành động của An vi phạm "Quy tắc Cờ Đỏ Bảo mật"**

Hành động của An được xem là một pha "tự tay bóp chân mình" kinh điển và vi phạm nghiêm trọng các quy tắc bảo mật dữ liệu vì những lý do sau:

- Kết hợp tai hại giữa "Anyone with the link" và quyền "Editor":
Chế độ Anyone with the link biến tài liệu thành một tấm biển quảng cáo công cộng ai cũng truy cập được mà không cần đăng nhập. Khi ghép thêm quyền Editor, bất kỳ ai tình cờ thấy link đều có thể toàn quyền thêm, sửa, hoặc... xóa sạch dữ liệu bên trong

- Lộ lọt thông tin nhạy cảm (Data Leak) ra ngoài tầm kiểm soát:
Danh sách số điện thoại khách hàng là tài sản cốt lõi của một Startup. Đem chia sẻ kiểu "thả nổi" thế này chẳng khác nào đưa chìa khóa két sắt cho người lạ. Việc link bị lọt lên các hội nhóm mạng xã hội (như nhóm Sale Bất Động Sản) là hệ quả tất yếu.

- Thiếu cơ chế kiểm soát định danh người dùng:
An chia sẻ cho cộng tác viên bên ngoài nhưng lại dùng link chung thay vì mời đích danh email cá nhân của họ. Điều này khiến An hoàn toàn mù tịt về việc ai đang xem, ai đang sửa, và ai là kẻ đã phá hoại dữ liệu.

## **PHẦN 2: Thiết lập lại cấu trúc phân quyền chuẩn cho An** 

***1. Mức quyền nào cho cộng tác viên?***

- Chỉ nên cấp quyền Commenter (hoặc giới hạn đúng phần việc nếu họ chỉ cần xem/đóng góp ý kiến) hoặc chỉ cấp Editor khi thực sự tin tưởng, nhưng bắt buộc phải giới hạn theo từng tài khoản email cụ thể thay vì cấp bừa bãi.

***2. Chế độ chia sẻ nào là an toàn 100%?***

- Phải chuyển từ chế độ mở sang Restricted (Hạn chế). Chỉ những người có email được An chủ động thêm vào danh sách cấp phép mới có thể mở được tài liệu. Tuyệt đối không dùng Anyone with the link cho các tài liệu chứa dữ liệu quan trọng, khách hàng hoặc tài chính.

***3.Cần bật tính năng gì để ngăn cộng tác viên tự ý mời thêm người lạ?***