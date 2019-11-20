---
title: HTML常用标签
permalink: HTML常用标签
comments: true
copyright: false
abstract: 交钱！
message: Enter password to read！
date: 2019-08-07 09:59:59
description: 常用HTML标签整理
categories:
- HTML
tags:
- HTML标签
top:
password:
---
## HTML标签
### HTML标题
```Html
<!DOCTYPE html>
<html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>Title</title>
  </head>
  <body>
    <h1>h1</h1>
    <h2>h2</h2>
    <h3>h3</h3>
    <h4>h4</h4>
    <h5>h5</h5>
    <h6>h6</h6>
  </body>
</html>
```

### 水平线/分割线
`<hr/>`
### 注释
`html注释：<!-- comment -->`
### 段落及换行
html段落`<p>`以及段落换行`<br/>`
```Html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<p>hello world</p>
<p>hello <br/> world</p>
<hr/>
</body>
</html>
```
### 文本格式化
|标签|描述|
|:-:|:-:|
|`<b>`|加粗|
|`<big>`   |大号字体   |
|`<em>`   |着重文字   |
|`<i>`   |斜体   |
|`<small>`   |小号字体   |
|`<sub>`   |下标   |
|`<sup>`   |上标   |

### 超链接
`<a>`比较重要的属性有两个，分别是href、target
href指定超链接地址
target指定打开方式
&emsp;\_blank  新页面打开
#### 普通链接
`<a href="http://www.baidu.com">百度</a>`
#### 邮件链接
标签最简式
`<a href="mailto:xxx@xx.com">联系站长</a>`
### 图像
使用格式：`<img src="url">`
&emsp;alt  定义当图片无法加载时，显示什么信息
&emsp;width 定义宽度，单位可以为像素  也可以为百分比
&emsp;height 定义高度
`< img src=“11.jpg” width="100px" height="200px" title="title" alt="图片无法显示，请刷新"/>`
### 表格
表格由`<table>`来定义，每行`<tr>` 有许多单元格`<td>`。表头可以使用`<th>`
`<table>`标签属性：
>border：表格边框属性；当使用border="1"设置边框时，会在所有td以及table上嵌套边框，当border加大时，只有table框会加粗。
cellspacing：单元格与单元格之间的间隙。当cellspacing="0"时，单元格之间的间隙为0，但边框线并不会合并。
☆☆合并边框的写法style="border-collapse:collapse;" 使用边框合并时，无需设置cellspacing。
cellpadding:单元格内边距，单元格中文字与单元格边框之间的距离。
width/height:表格的宽高
align：设置表格在父容器中的对齐方式 ，left/居左 center/居中 right/居右
☆☆当表格使用align属性时，相当于使表格浮动，可能会导致表格后面的元素受表格浮动影响，导致布局错乱。
bgcolor：背景色
background：背景图，后接相对路径。背景图和背景色同时生效时，图会覆盖背景色
bordercolor：设置边框颜色

在`<table>`中可以嵌入`<th>  <tr>  <td>`等标签
　　`<tr>`   定义行
　　`<th>`   定义表头
　　　　colspan  定义表头单元格可以横跨的列数。
　　　　rowspan  定义表头单元格横跨的行数
　　　　heardes  定义与表头单元格相关联的一个或者多个单元格。(html5新增)　　　
　　`<td>`   定义单元格
　　　　colspan  定义单元格可以横跨的列数。
　　　　rowspan  定义单元格横跨的行数
　　　　heardes  定义与单元格相关联的一个或者多个单元格。(html5新增)　　
表格固定高度：overflow-y:auto;
### 列表
无序列表`<ul>`
有序列表`<ol>`
```Html
<ul>
    <li>male</li>
    <li>female</li>
</ul>
<hr/>
<ol>
    <li>male</li>
    <li>female</li>
</ol>
```
