# picoCTF 2023 WEB
## 1.findme 
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337760106_164576173135850_978736351320606101_n.png?stp=dst-png_p206x206&_nc_cat=109&ccb=1-7&_nc_sid=aee45a&_nc_ohc=aJpOLkgAY-0AX8GR7r8&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSnaq8-Qya2QIFex7TtahqQs14n-w_bFf7VQAT69l9SJQ&oe=644927BD)

Mình đăng nhập bài tài khoản `test:test!` mình dùng `burpsuite` để bắt request.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336632297_719908109832224_8539978371072231178_n.png?stp=dst-png_p228x119&_nc_cat=111&ccb=1-7&_nc_sid=aee45a&_nc_ohc=YnJ3EIQogKIAX_04zsJ&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTbtgDtROC9ynItGfsGdpspykLAiR7pDxryW3Io2DSx-A&oe=64493ACD)

sau khi đăng nhập ta thấy có 2 request title là `flag` có 2 giá trị `cGljb0NURntwcm94aWVzX2Fs` `bF90aGVfd2F5XzI1YmJhZTlhfQ==`.
Chỗ này mình thử encode thì nhận được flag luôn.    
>flag: picoCTF{proxies_all_the_way_25bbae9a}

## 2.MatchTheRegex
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337356827_1178448419538562_3832145059405279901_n.png?stp=dst-png_p206x206&_nc_cat=110&ccb=1-7&_nc_sid=aee45a&_nc_ohc=6c4fmjGaKycAX_MaMrU&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdS2b4Gl82GOaCzuW2Yd-cJ7oNuL-YgUqMfcy7lQl4ierA&oe=64491B7D)

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337767577_608607411127162_5651826542348912289_n.png?stp=dst-png_p206x206&_nc_cat=102&ccb=1-7&_nc_sid=aee45a&_nc_ohc=mfGomiw2k4wAX-awCOO&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdToxC5yuR49xqq1suzV1gFIRxKpxP31sFUY_VriVGTmCw&oe=644928D9)

Bài này là họ bắt mình `bypass regex`.Mình để ý tới chỗ comment của họ `^p.....F!?`,mình nghĩ là input nhập vào phải match cái này.

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337125021_527534639314019_9127641666227625811_n.png?stp=dst-png_p206x206&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=I4i8QPK73vcAX_haenz&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdRmGayC8W-NSThsby4eoqS-S7U3oKXfhiyllZ5jBjg3WA&oe=64492562)

Mình nhập `picoCTF` thì nhận được flag luôn.
>Flag: picoCTF{succ3ssfully_matchtheregex_8ad436ed}

## 3.SOAP
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/336524100_925373732215592_6625368259157943177_n.png?stp=dst-png_s350x350&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=cGlQLSVCMvcAX_dayfT&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSulS6PvIN2PUAK-307vPrTcGW7GbKKyECak6w3o90Egg&oe=6449E15F)

Hint `XML external entity Injection`

Bắt request sai khi click details:

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/329719008_5990649014353000_4368733121751755251_n.png?stp=dst-png_s600x600&_nc_cat=100&ccb=1-7&_nc_sid=aee45a&_nc_ohc=ciZ29vmYJzcAX_18xaJ&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTWEU_UEvSkj8dEsAbW5y3uY1muHHUa0tYEvwJjHinAJg&oe=6449D717)

Bài này hint là `XML external entity Injection` nên mình thử chỉnh payload.
Mình thử payload ở đây [https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity](https://book.hacktricks.xyz/pentesting-web/xxe-xee-xml-external-entity).

![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337952078_595514919295008_2735216027573118110_n.png?stp=dst-png_s417x417&_nc_cat=101&ccb=1-7&_nc_sid=aee45a&_nc_ohc=YVvPRFZrH4MAX-evPaN&_nc_oc=AQnzqtg34d84gJ2zuuAjJyhwVda4KMLZLokqKfeJX8hpapJDad1-X153YE8OBN_i8cn16aSSAKbUYOhq96wiPOvu&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTK0HtL78nxRx3JBUfthmV5fkDEO2IYCGJmeFBYm1ygNg&oe=6449EB93)

> Flag : picoCTF{XML_3xtern@l_3nt1t1ty_4dbeb2ed} 

## 4.More SQLi
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337629154_949298189848345_1642424985710383697_n.png?stp=dst-png_p206x206&_nc_cat=100&ccb=1-7&_nc_sid=aee45a&_nc_ohc=e9mcZgelCRYAX91iGSZ&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdSorFo-TQe5KauiOt5dojG48vE-tyhEK2AIL9X-oKngHw&oe=6449DF84)

Mình thử tài khoản `test:test` nhận được:
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/338162331_178382934984565_8395621353215455166_n.png?_nc_cat=106&ccb=1-7&_nc_sid=aee45a&_nc_ohc=9Ffocnfv1WUAX9wUp9R&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdTCfnq-TnxAuOA3gEZGBEM8_7TkjfyiYkKiZundQ3esEA&oe=6449D56B)

Mình thử sqli với password `test' or 1=1 --` nhận được:
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/337033800_1631138503980052_4800427442349539660_n.png?stp=dst-png_p235x165&_nc_cat=108&ccb=1-7&_nc_sid=aee45a&_nc_ohc=bHFj_Vg7AwkAX_M8Hbl&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdT8S_xq04a4chjwkz93RWT6ULMyqyr1wpwtyZ8ks2McRw&oe=6449ED47)

Như vậy ở đây mình có thể sqli.
Tuy nhiên sau khi dùng `burpsuite` thì mình nhận được flag luôn.
![image](https://scontent.xx.fbcdn.net/v/t1.15752-9/338407261_207650418532705_374567366035620292_n.png?stp=dst-png_s480x480&_nc_cat=107&ccb=1-7&_nc_sid=aee45a&_nc_ohc=X_hp71EhIQUAX-VfjkC&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AdQoze5E1GkDIrhemk1ZDajqtNQQ_D7DYNuFi-N-lxeifw&oe=6449D78E)
> Flag: picoCTF{G3tting_5QL_1nJ3c7I0N_l1k3_y0u_sh0ulD_c8ee9477}


