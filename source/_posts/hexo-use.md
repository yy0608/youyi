---
title: hexo的基础命令，详细安装使用请看下一篇文章
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

### 更换电脑之后的仓库使用hexo
```
npm install hexo-cli -g

// 从仓库拉下代码，进入目录运行以下命令
npm install hexo -S
npm install
npm install hexo-deployer-git // 不要运行hexo init命令
```

### 莫名会出现的异常问题，修改_config.yml文件
```
https://github.com/yy0608/youyi.github.io

to

git@github.com:yy0608/youyi.github.io.git
```

More info: [Deployment](https://hexo.io/docs/deployment.html)
