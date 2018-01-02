---
title: github技术博客hexo使用
date: 2017-12-25 16:04:15
tags:
  - 技术博客
  - hexo
---
- 1.环境准备，[Git](http://gitforwindows.org/), [node.js](https://nodejs.org/en/), hexo(npm install hexo-cli -g)
```
设置npm镜像源
npm config set registry https://registry.npm.taobao.org --global
npm config set disturl https://npm.taobao.org/dist --global

设置yarn镜像源（此处用不到yarn）
yarn config set registry https://registry.npm.taobao.org --global
yarn config set disturl https://npm.taobao.org/dist --global
```
- 2.hexo使用
```
hexo init (初始化)
npm install (安装相关依赖)
hexo generate (生成相关静态页面)
hexo server (本地查看)
```
- 3.操作github(省略github账号创建)
```
1.创建仓库，仓库名字格式：yourname.github.io

2.本地生成ssh密钥，ssh-keygen -t rsa -C "你的邮箱地址"，按3个回车，密码为空。
  在C:\Users\Administrator.ssh下，得到两个文件id_rsa和id_rsa.pub

3.github添加ssh密钥，打开id_rsa.pub，复制全文。
  https://github.com/settings/ssh ，Add SSH key，粘贴进去

4.全局配置_config.yml，
  修改language: zh-Hans, type: git, repo: https://github.com/yy0608/youyi.github.io(此处为刚创建的仓库地址)
```
- 4.部署
```
hexo clean (清除缓存)
hexo generate (生成页面)
hexo deploy (上传仓库)

按照提示打开地址，可以配置自己的域名，点击 Github 项目上的 Settings > GitHub Pages，需要在域名后台配置域名解析

github上新建一个CNAME文件，参考地址https://www.jianshu.com/p/cea41e5c9b2a
```
- 5.以上基本就完成了，下面是主题的修改，本站使用的是排名第一的next主题
```
1.下载要使用的主题地址，下载后的地址为，themes目录
  git clone https://github.com/iissnan/hexo-theme-next themes/next

2.修改全局的_config.yaml的theme配置项为next

3.如果要修改主题的细节配置，可参考http://theme-next.iissnan.com/，
  主要是修改主题目录下的_config.yaml文件
```

# &nbsp;
> *参考文章，[手把手教你建github技术博客](https://www.jianshu.com/p/701b1095da11)*