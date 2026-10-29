**Phần 1: Phân tích lỗ hổng**

**Câu 1. Xác định các dòng log gây rò rỉ dữ liệu**

-   Dòng 1 (08:15 T2): File campuspay_beta_users.xlsx được cấp quyền Editor và phạm vi Anyone with the link. Đây là lỗi nghiêm trọng vì bất kỳ ai có đường dẫn đều có thể xem, sửa, xóa hoặc chia sẻ tiếp file chứa thông tin sinh viên.
    
-   Dòng 3 (14:00 T2): File slide_pitch_v8.pptx được cấp quyền Commenter và phạm vi Anyone with the link. Người có đường dẫn có thể xem và bình luận, nhưng không thể sửa trực tiếp nội dung file gốc. Cần kiểm tra xem slide có chứa thông tin nội bộ hay không.
    
-   Dòng 4 (16:45 T2): File campuspay_beta_users.xlsx chỉ cấp quyền Editor cho Email C theo chế độ Restricted. Phạm vi này hạn chế người ngoài truy cập, nhưng cần kiểm tra xem Email C có thực sự cần quyền chỉnh sửa hay không.
    
-   Dòng 2 (09:30 T2): File campuspay_api_keys.txt được cấp quyền Viewer cho Email B theo chế độ Restricted. Cấu hình này hạn chế người truy cập, nhưng file chứa khóa API nên cần bảo vệ nghiêm ngặt hơn.
    
-   Dòng 5 (10:00 T3): Email người lạ truy cập file campuspay_api_keys.txt là dấu hiệu bất thường, cần điều tra thêm.
    

**Kết luận:** Dòng 1 là nguồn gây rò rỉ dữ liệu khách hàng rõ ràng nhất vì kết hợp quyền Editor với Anyone with the link. Dòng 3 cũng có nguy cơ lộ thông tin nếu slide chứa dữ liệu nội bộ.

**Câu 2. Ba giả thuyết giải thích việc người lạ truy cập file API**

1.  Email người lạ đã được cấp quyền trực tiếp trước đó nhưng chưa được ghi lại trong bảng log đang xem.
    
2.  Một tài khoản được cấp quyền đã bị chiếm quyền sử dụng, khiến người lạ có thể đăng nhập và truy cập file.
    
3.  File đã được chia sẻ, sao chép hoặc di chuyển sang một vị trí khác có phạm vi truy cập rộng hơn, nên cần kiểm tra lịch sử hoạt động và quyền kế thừa.
    

**Phần 2: Sửa lỗi và nghiệm thu**

**Câu 3. Bảng cấu hình quyền chia sẻ đúng chuẩn**
|Tên file|Quyền đúng|Phạm vi đúng|
|:----:|:----:|:----:|:----:|
|ampuspay_beta_users.xlsx|Viewer hoặc Editor nếu thực sự cần chỉnh sửa|Restricted|
|campuspay_api_keys.txt|Viewer nếu bắt buộc phải đọc; tốt nhất không chia sẻ rộng|Restricted|
|slide_pitch_v8.pptx|Commenter|Restricted|
|campuspay_beta_users.xlsx (bản kiểm thử)|Editor cho người phụ trách chỉnh sửa|Restricted|

**Giải thích:**

-   File dữ liệu sinh viên chứa thông tin cá nhân nên chỉ người có nhiệm vụ mới được truy cập.
    
-   File API chứa thông tin nhạy cảm, cần giới hạn người truy cập và không chia sẻ công khai.
    
-   File thuyết trình chỉ nên cấp quyền bình luận cho người góp ý và giới hạn phạm vi chia sẻ.
    
-   Người kiểm thử chỉ được cấp quyền chỉnh sửa dữ liệu khi công việc thực sự yêu cầu.
    

**Câu 4. Vô hiệu hóa link cũ và sử dụng Set Expiration Date**

-   Cách xử lý nhanh: Mở Google Drive, chọn file, vào Share, đổi General access từ Anyone with the link sang Restricted và xóa những người không còn cần quyền truy cập.
    
-   Kiểm tra lại danh sách người được chia sẻ để bảo đảm không còn tài khoản lạ.
    
-   Tính năng Set Expiration Date cho phép đặt ngày hết hạn truy cập đối với người được chia sẻ đủ điều kiện. Nên dùng khi chia sẻ file tạm thời cho người kiểm thử, cộng tác viên hoặc đối tác bên ngoài.
    
-   Nếu phát hiện rò rỉ dữ liệu, cần thu hồi quyền truy cập ngay, không chờ đến ngày hết hạn.
    

**Kết luận:** Em cho rằng nguyên nhân chính là chia sẻ file dữ liệu sinh viên ở chế độ công khai và cấp quyền chỉnh sửa quá rộng. Nhóm cần chuyển các file nhạy cảm sang Restricted, giới hạn quyền theo nhiệm vụ và thường xuyên kiểm tra lịch sử chia sẻ để tránh sự cố tương tự.
