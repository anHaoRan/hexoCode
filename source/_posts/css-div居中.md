---
title: css div居中
date: 2017-04-14 16:31:56
tags: css
toc: true
---

使用css3将一个div水平和垂直居中显示
使用css3将一个div水平和垂直居中显示

方案一：

div绝对定位水平垂直居中【margin:auto实现绝对定位元素的居中】，
<!--more-->
代码两个关键点：1.上下左右均0位置定位；

　　　　　　　　2.margin: auto; 其width、height如何更改都是居中显示的，兼容性可以,IE7及之前版本不支持

复制代码
 .div1{
     width: 100px;
     height: 100px;
     border: 4px solid red;
     position: absolute;
     left:0;
     right:0;
     top: 0;
     bottom: 0;
     margin: auto;
     /*50%为自身尺寸的一半*/
 }
代码

优点：

1.支持跨浏览器，包括IE8-IE10.

2.无需其他特殊标记，CSS代码量少

3.支持百分比%属性值和min-/max-属性

4.只用这一个类可实现任何内容块居中

5.不论是否设置padding都可居中（在不使用box-sizing属性的前提下）

6.内容块可以被重绘。

7.完美支持图片居中。

缺点：

1.必须声明高度（查看可变高度Variable Height）。

2.建议设置overflow:auto来防止内容越界溢出。（查看溢出Overflow）。

3.在Windows Phone设备上不起作用。

浏览器兼容性：

Chrome,Firefox, Safari, Mobile Safari, IE8-10.

绝对定位方法在最新版的Chrome,Firefox, Safari, Mobile Safari, IE8-10.上均测试通过。

方案二：

div绝对定位水平垂直居中【margin 负间距】

此方案代码关键点：
1.必需知道该div的宽度和高度，
2.然后设置位置为绝对位置，
3.距离页面窗口左边框和上边框的距离设置为50%，这个50%就是指页面窗口的宽度和高度的50%，
4.最后将该div分别左移和上移，左移和上移的大小就是该DIV宽度和高度的一半。

复制代码
.div1{

    width: 100px;
    height: 100px;
    border: 4px solid red;
    position: absolute;
    text-align: center;
    left:50%;
    top: 50%;
    margin: -50px 0 0 -50px;
    /*50%为自身尺寸的一半*/
}
亦可写成：

.div1{
  
    width: 100px;
    height: 100px;
    background-color: green;
    position: absolute;

    text-align: center;

    left:50%;
    top: 50%;
    margin-left: -50px; /*  width/2  */
    margin-top: -50px; /*  height /2 */  

}

这或许是当前最流行的使用方法。

测试表明，这是唯一在IE6-IE7上也表现良好的方法。

优点：

1. 良好的跨浏览器特性，兼容IE6-IE7。

2. 代码量少。

缺点：

1. 不能自适应。不支持百分比尺寸和min-/max-属性设置。

2. 内容可能溢出容器。

3. 边距大小与padding,和是否定义box-sizing: border-box有关，计算需要根据不同情况。

方案三：

div绝对定位水平垂直居中【Transforms 变形】

这是最简单的方法，不仅能实现绝对居中同样的效果，也支持联合可变高度方式使用。内容块定义transform: translate(-50%,-50%)  必须加上
top: 50%; left: 50%;

.div1{
   
    width: 200px;
    height: 200px;
    background-color: pink;
    position: absolute;
    text-align: center;
    left:50%;
    top: 50%;
    /*-webkit-transform: translate(-50%,-50%);*/
    /*-ms-transform: translate(-50%,-50%);*/
    transform: translate(-50%,-50%);
}

优点：

1.  内容可变高度

2.  代码量少
缺点：
1.  IE8不支持
2.  属性需要写浏览器厂商前缀
3.  可能干扰其他transform效果
4.  某些情形下会出现文本或元素边界渲染模糊的现象

若只是水平（方向）居中：

复制代码
.div1{
    width: 100px;
    height: 100px;
    border: 4px solid red;
    text-align: center;
    margin: 0  auto;
    /*50%为自身尺寸的一半*/
}

css3不定宽高水平垂直居中

只要三句话就可以实现不定宽高水平垂直居中。

1 justify-content:center;//子元素水平居中
2 align-items:center;//子元素垂直居中
3 display:-webkit-flex;
在父级元素上面加上上面3句话，就可以实现子元素水平垂直居中。
