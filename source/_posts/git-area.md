---
title: 局域网内git的搭建和使用
tags:
  - 局域网git
abbrlink: 63418
date: 2018-01-19 17:46:20
---
##### 单个项目小团队在局域网内开发，现在是直接编辑共享文件。主要问题有
- 1.每次保存会直接上传到目标电脑，开发并不需要每次上传
- 2.没有版本控制和提交记录，无法回滚

###### 一、共享目标电脑的文件夹（类似远程服务器）
###### 二、该文件夹下可以有不同项目文件夹，通过git init初始化
###### 三、其他电脑通过git clone \\192.168.2.167\dirname\project_name进行拷贝
###### 四、其他操作同[git基本操作](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)
###### 五、如果ip变动，使用如下方法进行修改
```
// 修改
git remote set-url origin [url]

// 先删除后添加
git remote rm origin
git remote add origin [url]
```
##### 通过git，将目标电脑代码拷贝到本地进行开发，只需要在提交时使用网络