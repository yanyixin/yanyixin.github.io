---
title: Icomoon 的使用
date: 2016-06-14 
tags:
---
IcoMoon是一个可以通过个性化设置来创建自定义图标（字体）的免费生成器。
上面有600+的免费海量图标集，还可以按照自己的需求来定制，而且兼容IE6+以及各种手机设备。前端工程师们可以自己来生成图标，快速简单方便。

<!--more-->

1、打开主页面（https://icomoon.io/app/#/select）
![](http://upload-images.jianshu.io/upload_images/1561693-d6dc72079363c081.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2、![](http://upload-images.jianshu.io/upload_images/1561693-5052ef7c17abc6dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)新建一个图集

3、添加图片
 a、可以把自己的.svg拖进来
     ![](http://upload-images.jianshu.io/upload_images/1561693-f04992e0543e5076.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
b、也可以点击[Add Icons From Library](https://icomoon.io/app/#/select/library) 来选择网站现有的图标
![](http://upload-images.jianshu.io/upload_images/1561693-e057f43801b911c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、生成字体

          点击选中的图标之后，背景会变成白色。
![](http://upload-images.jianshu.io/upload_images/1561693-989f03c89c26620a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果想要编辑选中的图标，就可以点击上方的Edit，然后再点击选中的图标，就可以进行编辑啦。
![](http://upload-images.jianshu.io/upload_images/1561693-507ab0683ea530ec.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以对图标进行旋转操作和上下左右移动的操作
![](http://upload-images.jianshu.io/upload_images/1561693-4b6e49ce333b3753.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/1561693-8678d92d834a50e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

编辑完成之后点击Generate Font F 生成字体

![](http://upload-images.jianshu.io/upload_images/1561693-fffd797cb08a6b69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里可以自己命名图标的名字
![](http://upload-images.jianshu.io/upload_images/1561693-ce40d62e178a9e64.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后就可以点击Download来下载文件啦。
![](http://upload-images.jianshu.io/upload_images/1561693-bfec463d588d0d27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下载到本地之后解压文件，就会得到以下的文件：
![](http://upload-images.jianshu.io/upload_images/1561693-379baa8735b8fcd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
5、在网页上使用。

     将fonts文件夹里面的全部文件和style.css里面的内容放到相应的项目中。
![](http://upload-images.jianshu.io/upload_images/1561693-bdf2fe0f82c87e42.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![](http://upload-images.jianshu.io/upload_images/1561693-0f45a99955f2aed4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

根据css生成的类名，应用在html文件里就可以添加字体啦。
可以通过font-size来设置图标的大小，通过color来设置图标的颜色。

<span class="icon-checkmark"></span>

.icon-checkmark{
  font-size: 30px;
  color:pink;
}
![](http://upload-images.jianshu.io/upload_images/1561693-b046e21fb730a389.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当当当当~ 完成啦。总体来说还是很方便的，大小颜色任意设置。大家快用起来吧~