# 1.PHP - Loose Comparison(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch55/index.php](http://challenge01.root-me.org/web-serveur/ch55/index.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331946676_720451759621314_1492044808517406992_n.png?stp=dst-png_s480x480&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=M6h4ZZHKV-0AX-FZe83&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQ5ElleU1EnGZC0UBI7q7_ornT9vp-kvv0b_VYuxu_Z0g&oe=641A9BB0)

Bài này họ cho mình source code:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330869484_719543483152599_2010351930084355003_n.png?stp=dst-png_s261x260&_nc_cat=104&ccb=1-7&_nc_sid=aee45a&_nc_ohc=HQa1XrhDdAsAX86x2JK&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSmPf6v6NNi3r_PuNTPArwLkO67mpamBJOW0luEGoKX-A&oe=641AF889)

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
Well done! Here is your flag: F34R_Th3_L0o5e_C0mP4r15On*  

# 2.PHP - type juggling(Root me)
Link challenge: [http://challenge01.root-me.org/web-serveur/ch44/](http://challenge01.root-me.org/web-serveur/ch44/) 

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/331912129_1629596314165218_4870990480733112814_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=uBs0ZFy6__sAX_0Pkb3&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRU_GaHm0NwlxVaDTDMxlEc4ogGCFyrPS57aODMdBLpkA&oe=641AED98)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330885928_1156438515073510_5982228528780955963_n.png?stp=dst-png_p206x206&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=v70me0W2r4MAX_i0MAd&_nc_oc=AQmPy47u_zV-IqM6KkDapSo4b9y_n_H3emXstRdBurTEMqknXJrfmhf-pl6V4T0nYcLW3wDPvkZ3ZlqxzVitUaW3&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRZQJOUZrujrTFB9kfz-TgMYFvAjNFO11XVcYDOGcUaHA&oe=641AE96E)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/330903896_9573023332738457_5743595042738473553_n.png?stp=dst-png_s403x403&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=ahUlFJpUSmsAX9oOWZF&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT66mI-uHgqHA6oh8MYLK4q7PzjwZsq6VRbi5xSnVNa4g&oe=641AD28B)
