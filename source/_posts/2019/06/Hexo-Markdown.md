---
title: Hexo Markdown
permalink: Hexo Markdown
date: 2019-06-15 20:15:05
comments: true
copyright: true
description: 常用Hexo指令、Markdown格式
categories:
- Hexo
tags:
- Hexo
- 主题
- 文章密码
- Markdown
top:
password:
abstract: 交钱！
message: Enter password to read！
---

## Hexo指令
### 新建文章
```
$ hexo new 布局 "文章名"
$ hexo clean //清除静态页面缓存（清除 public 文件夹)         
$ hexo g     //在本地生成静态页面（生成 public 文件夹）        
$ hexo s     //启动本地服务 http://localhost:4000，进行预览调试           
$ hexo d     //远程部署，同步到 GitHub         

$ npm install hexo-deployer-git --save    //自动部署
$ hexo clean && hexo g && hexo d          //发布
```
## Markdown格式
### 首行缩进
    &ensp; //相当于1个空格  
    &emsp; //相当于2个空格，1个汉字  
分段: 两个回车
换行: 两个空格 + 回车
标题: # ~ ######，#号的个数表示几级标题，即表示一级标题到六级标题
### 强调
```
*斜体* | **加粗** | ***斜体加粗***  
_斜体_ | __加粗__ | ___斜体加粗___  
~~删除线~~
```
引用: > 注意后面紧跟个空格
```
  > 以下为引用效果
  继续引用
  > >二级引用
```
> 以下为引用效果
继续引用
> >二级引用

表格: - 和 | 分割行和列 ， : 控制对齐方式
```
| 0 | 1 | 2|
| :- | -: | :-: |
| 0 | 1 | 2 |
| 0 | 1 | 2 |
| 0 | 1 | 2 |
```
| 0 | 1 | 2|
| :- | -: | :-: |
| 0 | 1 | 2 |
| 0 | 1 | 2 |
| 0 | 1 | 2 |

代码块: 四个空格开头或三个`
链接: `[文字](链接地址)`
邮件链接：`[xxx](mailto:xxx@xx.com.cn)`
图片: ![图片说明]\(图片地址)，地址可以是本地路径，也可以是网络地址
列表: * ， + ， - ， 1. ，选其中之一，注意后面紧跟个空格
### 设置字体段落格式

```
<center>居中</center>
<font color="#FF0000"> 设置颜色 </font>  
<font size=6> 设置大小 </font>
<font size=5 color="#FF0000"> 设置颜色和大小</font>
```

<center>居中</center>

<font color="#FF0000"> 设置颜色 </font>  
<font size=6> 设置大小 </font>
<font size=5 color="#FF0000"> 设置颜色和大小</font>
### 引用站内文章
在写文章的过程中，有时候需要引用站内的其他文章。可以通过内置的标签插件的语法post_link来实现引用。
`{% post_link 文章文件名（不要后缀） 文章标题（可选） %}`

## Hexo主题设计
### 头像
头像配置文件位于：主题配置文件中的 avatar下
### 版权信息
版权信息的配置文件位于：next\layout\\\_macro\my-copyright中
### 添加文章密码
#### 方法一(测试可用)
##### 安装hexo-blog-encrypt
根目录的package.json文件夹中添加：
```
"hexo-blog-encrypt": "2.0.*"
```
然后在命令行输入：
```
npm install
```
根目录下的_config.yml文件中添加：
```
# Security
encrypt:
    enable: true
```
##### 使用
在需要加密的文章头部写入password：
```
password: abc123
abstract: Welcome to my blog, enter password to read.
message: Welcome to my blog, enter password to read.
```
#### 方法二
在 themes->next->layout->\\\_partials->head.swig 中添加下面内容
```JavaScript
<script>
    (function(){
        if('{{ page.password }}'){
            if (prompt('请输入文章密码') !== '{{ page.password }}'){
                alert('密码错误,交钱还是跑路？');
                history.back();
            }
        }
    })();
</script>
```
然后在文章头部加入password。
### 添加边栏背景图
在  themes\next\source\css\\\_custom\custom.styl文件中
添加
```JavaScript
.sidebar {
 background: url([https://ws2.sinaimg.cn/large/006tKfTcly1fq2wrm6g3cj309i0hq749.jpg](https://ws2.sinaimg.cn/large/006tKfTcly1fq2wrm6g3cj309i0hq749.jpg "https://ws2.sinaimg.cn/large/006tKfTcly1fq2wrm6g3cj309i0hq749.jpg")) no-repeat !important;
 background-size: cover !important;
 position: fixed !important;
 right: 0 !important;
 top: 0 !important;
 bottom: 0 !important;
}

```
### 网易云音乐插件
主题文件夹
`layout\\\_custom\\sidebar.swig`

### Warning: LF will be replaced by CRLF
`git config --global core.autocrlf false //禁用自动转换`
### 更改文章全局属性(标题)
`D:\GitProject\FirstHexo\themes\next\source\css\_common\components\post\post.styl`

### 文章模板
在新建文章时，Hexo 会根据 scaffolds 文件夹内相对应的文件来建立文件，例如：hexo new photo "My Gallery"，在执行这行指令时，Hexo 会尝试在 scaffolds 文件夹中寻找 photo.md模板，并根据其内容建立文章，默认使用\_config.yml 中的 default_layout 参数post代替
