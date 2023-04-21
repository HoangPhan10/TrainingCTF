# Server Side Request Forgery
## 1. SSRF là gì?

![image](https://media.geeksforgeeks.org/wp-content/uploads/20210326225503/RRRBlueandRedFlatGraphicMoonCratersAstronomyClassroomGroupWork1-660x371.png)

- SSRF hay còn gọi là tấn công yêu cầu giả mạo từ phía máy chủ cho phép kẻ tấn công gửi các yêu cầu HTTP từ phía máy chủ ứng dụng đến các nguồn tài nguyên bên ngoài mà không cần trực tiếp kết nối tới các tài nguyên đó.Điều này có thể dẫn đến các cuộc tân công như truy cập trái phép vào các hệ thống nội bộ của tổ chức,tấn công các máy chủ bên ngoài hoặc đánh cắp dữ liệu.
- Kẻ tấn công gửi các yêu cầu HTTP chứa địa chỉ URL bằng cách lợi dụng một lỗ hổng trong ứng dụng web để xác định các thông tin nhạy cảm như địa chỉ IP,tên miền hoặc URL. 
## 2.Hậu quả
- Một cuộc tấn công SSRF thành công có thể dẫn đến cách hành động truy cập dữ liệu trái phép trong một tổ chức,trong chính ứng dụng đó hoặc trong các hệ thống back-end khác mà ứng dụng đó có thể giao tiếp.
- Trong một số trường hợp thì có thể dẫn tới RCE
## 3.Một số cách khai thác
#### 3.1 SSRF tấn công quét mạng nội bộ

![image](https://whitehat.vn/attachments/ssrf-png.10173/)

- Quét toàn bộ mạng nội bộ để tìm kiếm các máy chủ đích để tấn công hoặc khai thác.
- Kẻ tấn công tận dụng lỗ hổng SSRF để truy cập các trang web vốn dành cho tài khoản có quyền quản trị viên hoặc chỉ có thể truy cập từ phía local server.Local server thường có địa chỉ URL dạng: `http://127.0.0.1` hoặc `http://localhost`.

#### 3.2 SSRF tấn công điều hướng đến các máy chủ khác trong mạng nội bộ
- Các hệ thống này thường không được truy cập trực tiếp từ public server,để tấn công được các server này chúng ta cần biết chính xác domain hoặc địa 
chỉ IP của chúng thường dùng phương pháp `Brute Force`.

#### 3.3 SSRF tấn công đến các máy chủ bên ngoài
- Gửi các yêu cầu đến máy chủ bên ngoài để thực hiện các cuộc tấn công phức tạp hơn chẳng hạn như tấn công DDOS hoặc truy cập vào các thông tin nhạy cảm từ các máy chủ bên ngoài.
#### 3.4 SSRF tấn công RCE
- Kẻ tấn công có thể sử dụng lỗ hổng SSRF để khai thác các lỗ hổng của phần mềm không được bảo vể đầy đủ để thực thi mã độc hoặc các tác vụ độc hại trên máy chủ ứng dụng.
- Thực thi các lệnh shell trên máy chủ ứng dụng và kiểm soát hoàn toàn hệ thống.
#### 3.5 SSRF tấn công Blind SSRF
- Kẻ tấn công có thể khai thác lỗ hổng SSRF ma fkhoong cần nhận được bất kỳ phản hồi nào từ server.Thay vào đó kẻ tấn công sử dụng các phương pháp khác như phân tích log,kiểm tra thời gian phản hồi hoặc kiểm tra một só chỉ số khác để xác định kết quả của yêu cầu và trích xuất thoong tin nhạy cảm từ các tài nguyên ngoài.
- Nó khó khai thác hơn nhưng có thể dẫn tới RCE

### 4.Các biện pháp phòng chống tấn công SSRF
- Kiểm tra và lọc đầu vào: hạn chế người dùng nhập các URL không xác định hoặc các thông tin không tin cậy vào các trường đầu vào.
- Sử dụng whilelist: để giới hạn các địa chỉ IP hoặc tên miền có thể được truy cập.
- Sử dụng một máy chủ trung gian: giúp kiểm soát và giới hạn các yêu cầu truy cập bên ngoài,giảm thiểu rủi rỏ tấn công SSRF.
- Hạn chế quyền truy cập: đảm bảo rằng nó chỉ có thể truy cập các tài nguyên cần thiết


