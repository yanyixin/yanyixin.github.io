---
title: 了解Retina屏幕和Retina屏幕下图片的显示问题
date: 2017-07-30 20:38:18
tags:
---
今天被问到关于像素的问题，怒补了一下。

   `retina`的原意是`视网膜`，retina屏就是通过技术把更多的像素点压缩到屏幕里，使肉眼无法识别屏幕上的单个像素。

那么问题来了，如何分辨retina屏呢？

<!--more-->

首先了解几个名词：
##设备像素比：window.devicePixelRatio（简称：dpr）

`设备像素比（dpr） = 物理像素／设备独立像素`

##物理像素
      物理像素就是屏幕上的最小的物理部件，就是像素点，比如iphone6的物理像素就是750x1334

##设备独立像素
  也叫密度无关像素。可以认为是计算机坐标系统中得一个点，这个点代表一个可以由程序使用的虚拟像素(比如: css像素)，然后由相关系统转换为物理像素。比如iphone6的设备独立像素就是375x667px

##每英寸像素(pixel per inch)：
  表示每英寸屏幕上所拥有的像素个数。ppi越大，屏幕像素密度越大。

以iphone6为例：

  iphone6的dpr = `1334/667 = 2`

dpr为2是什么意思呢，看下图
![retina-web-3.jpg](http://upload-images.jianshu.io/upload_images/1561693-79f539a68341caf7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在普通屏幕上，1px的设备独立像素对应1物理像素。在retina屏幕下，1px的设备独立像素对应4物理像素。

##位图像素
  一个位图像素是栅格图像(如：png, jpg, gif等)最小的数据单元。

![TB12ALnIpXXXXb1XVXXXXXXXXXX.jpg](http://upload-images.jianshu.io/upload_images/1561693-fed262c4cabf8973.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在普通屏幕下，1个位图像素对应着1个物理像素，图片可以完美的显示。可是在retina屏幕下，1个位图像素对应着4个物理像素，由于位图像素不可以再分割，所以图片就会只能就进取色，导致图片模糊。
如何来处理这个问题呢。比如一个`200 x 300`的图片，如果想在retina屏幕上清晰显示的话，就要提供一个`400 x 600`的 `2倍图片`（@2x），这样的话，1个位图像素就会对应上retina屏上的1个物理像素。图片就不会模糊啦。