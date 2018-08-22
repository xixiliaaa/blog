---
layout: post
title: "phpqrcode生成二维码"
description: phpqrcode生成二维码
date: 2018-08-22
categories: php
tags: [php, phpqrcode, 二维码]
---

下载到最新版本：[地址](https://sourceforge.net/projects/phpqrcode/files/)。解压后，只需要使用phpqrcode.php文件即可，把文件复制到当前目录下面。

```php
include 'phpqrcode.php';   
$errorLevel="L";
$size="16";
QRcode::png('http://www.toabao.org/',false,$errorLevel,$size);   
```

这样就可以生成二维码了，实际上在png这个方法里还有几个参数需要使用。 

第一个参数$text，就是上面代码里的URL网址参数，

第二个参数$outfile默认为false，即默认不生成文件，只将二维码图片返回，否则需要给出存放生成二维码图片的路径

第三个参数$level默认为L，这个参数可传递的值分别是L(QR_ECLEVEL_L，7%)，M(QR_ECLEVEL_M，15%)，Q(QR_ECLEVEL_Q，25%)，H(QR_ECLEVEL_H，30%)。这个参数控制二维码容错率，不同的参数表示二维码可被覆盖的区域百分比。

　　利用二维维码的容错率，我们可以将头像放置在生成的二维码图片任何区域。

第四个参数$size，控制生成图片的大小，默认为4

第五个参数$margin，控制生成二维码的空白区域大小

第六个参数$saveandprint，保存二维码图片并显示出来，$outfile必须传递图片路径。

　　大家可以根据自己的需求来设置生成二维码的参数。