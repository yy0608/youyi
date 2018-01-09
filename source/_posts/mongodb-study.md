---
title: mongodb学习
date: 2018-01-04 17:26:37
tags:
  - mongodb
---
- 1.安装。使用chocolatey安装，-y是默认提示自动确认
```
choco install mongodb -y
```
- 2.将mongodb安装后的bin目录添加到系统环境变量
- 3.创建c:\data\db和c:\data\log文件夹
- 4.启动
```
mongodb --dbpath c:\data\db
```
- 5.命令行中执行mongo语句，打开新命令行窗口
```
mongo
```