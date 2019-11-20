---
title: css常用属性
permalink: css常用属性
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-07-05 13:54:12
description: 常用的css属性介绍
categories:
- css
tags:
- css属性
top:
password:
---
## 文本设置
```css
font: italic bold 36px 宋体; //顺序不可变
font-wight: 100-900、bold（加粗）
font-size: 10px; // 	12px(12像素)、50%、larger、small
font-style: initial（初始）、italic（斜体字）、normal（默认）、oblique（倾斜）
font-family: "微软雅黑" // 宋体
text-align: center; //横向排列  left、right 和 center
line-height: 200px; //文本行高 通俗的讲，文字高度加上文字上下的空白区域的高度 50%:基于字体大小的百分比
vertical-align:-4px; //设置元素内容的垂直对齐方式 ,只对行内元素有效，对块级元素无效
text-indent: 150px; //首行缩进
letter-spacing: 10px; //字母间隙
word-spacing: 20px; //单词间隙
text-transform: capitalize; //单词大写
```
## 背景属性
```css
background-color: cornflowerblue; //背景颜色
background-image: url('1.jpg'); //背景图片
background-repeat: no-repeat/repeat-x/repeat=t; //(默认铺满，不重复，x重复，y重复)
background-size:600px 250px //大小
background-position: right top（20px 20px）;//(横向：left center right)(纵向：top center bottom) //简写：
<body style="background: 20px 20px no-repeat #ff4 url('1.jpg')">
<div style="width: 300px;height: 300px;background: 20px 20px no-repeat #ff4 url('1.jpg')">
```
### 颜色属性
```css
<div style="color:blueviolet">ppppp</div> //颜色少
<div style="color:#ffee33">ppppp</div>  //百度颜色代码可以查询更多
<div style="color:rgb(255,0,0)">ppppp</div> //三原色 红绿蓝256级
<div style="color:rgba(255,0,0,0.5)">ppppp</div> //比上一个 加了一个透明度
```
