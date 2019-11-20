---
title: git command
permalink: git command
comments: true
copyright: true
abstract: 交钱！
message: Enter password to read！
date: 2019-11-20 18:10:47
description:
categories:
- Git
tags:
top:
password:
---
<center>写在前面</center>

<!--more-->

git配置
`git config --global user.name "Your Name"`

`git config --global user.email "email@example.com"`

创建本地仓库

|command|说明|
|:-|:-:|
git init| 在指定路径下执行
git add |提交文件到暂存区，多个文件空格分开
git commit| 提交到仓库分支
git commit -m "说明"| 添加本次提交的说明便于查看更改记录
git log |历史版本信息 Git中版本用HEAD做标识，当前版本为HEAD，上一个版本是HEAD^
git reset --hard HEAD^ | 退回到上个版本
git reset --hard commit_id|  HEAD 可用commit id前几位
git reflog| 查看之前所有版本信息
git status |查看状态
git diff HEAD -- file |查看工作区与版本库中的区别
git checkout -- file  |将工作区恢复到暂存区或版本库中的内容
git reset HEAD file | 将暂存区恢复到版本库的内容
git rm file | 删除文件
git remote add origin git@github.com:yourgithubname/Repositoryname.git |关联远程库
git push -u origin master |将本地仓库推送到远程仓库master分支并关联本地master分支
git push origin master  |推送到远程master分支
git clone git@github.com:githubname/Repositoryname.git |clone到本地
git branch bra | 创建bra分支
git checkout bra | 切换到bra分支
git checkout -b bra|等效于上面两条指令
git branch | 查看分支，当前分支用*标识，切换分支后提交到当前分支
git checkout master |切换到master分支
git merge  |合并指定分支到当前分支
git branch -d bra | 删除bra分支
git switch -c bra | 创建并切换分支
git switch master | 切换到master分支
