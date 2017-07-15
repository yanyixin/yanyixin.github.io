---
title: CSS 实现内容垂直居中
date: 2016-06-13
tags:
---

我们经常遇到的问题有实现 `css` 垂直居中，一个效果的实现可以有多种方法，了解每一个方法的原理也可以帮我们更快速的定位在我们需要的场景中需要使用哪种方法。接下来我就把我知道的一些方法和在网上总结的一些办法实现一下，俗话说：好记性不如烂笔头。写下来才能记忆深刻。

<!--more-->

首先： 实现水平居中有以下方法。如果是内联元素，设置父元素 `text-align :center`；
如果是块状元素：`margin: auto`;

接下来看垂直居中。

44年前我们把人送上月球，但在CSS中我们仍然不能很好实现垂直居中——@James Anderson

1、文本水平垂直居中
```
.outer{
  background-color: hotpink;
  margin:auto;
  height: 100px;
  width:200px;
}
.text{
  text-align: center;
  line-height: 100px;
}
```
![](http://upload-images.jianshu.io/upload_images/1561693-eee9e72c4e53e06e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、盒模型水平垂直居中

垂直居中在现在的需求上还是经常需要的，看似简单，可是当涉及到尺寸不固定的情况时，就变的非常棘手了。
```
<div class="box">
  <div class="outer"></div>
</div>
```
绝对定位的方法：

要求具有固定的宽度和高度：
```
.outer{
  position: absolute;
  top:50%;
  left:50%;
  margin-left: -100px;
  margin-top: -50px;
  width:200px;
  height: 100px;
  background-color: pink;
}
```
先把元素的左上角固定在正中心，然后再利用 `margin` 把左上角向左方和上方移动本身宽度的一半，这样就实现了垂直居中。
分析：需要给元素一个固定的尺寸，灵活性不是很高。

![](http://upload-images.jianshu.io/upload_images/1561693-d5a5bc97bf101ee0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果可以让元素按照自身尺寸的百分比来移动，那元素的尺寸就可以不用固定了。于是采用了 `transform` 中的 `translate( )` ；
```
.outer{
  position: absolute;
  top:50%;
  left:50%;
  transform:translate(-50%,-50%);
  background-color: hotpink;
}
```
![](http://upload-images.jianshu.io/upload_images/1561693-d935c285533b4c27.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

绝对定位不是一个很好的选择，因为对整体布局的影响还是很大的。

Flex布局
通过 `Flex` 布局可以很轻松的实现垂直居中
```
.box{
  display: flex;
  width:800px;
  height:500px;
  background-color: bisque;
  margin:100px auto;
}
.outer{
  background-color: hotpink;
  margin:auto;
}
```
![](http://upload-images.jianshu.io/upload_images/1561693-cf71c3fc114e94a1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

`flex` 的兼容性![](http://upload-images.jianshu.io/upload_images/1561693-b3e4fa77e390df94.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)