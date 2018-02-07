---
title: nginx配置多端口多域名，并配置https
date: 2018-02-06 18:12:57
tags:
  - nginx配置
---
###### 一、本文介绍使用nginx多个域名配置一个服务器的多个端口，nginx安装自行搜索，[多域名配置](https://segmentfault.com/a/1190000004453295)， [ssl参考文章](https://www.jianshu.com/p/9523d888cf77)
###### 二、启动目录和配置目录
```
// 启动目录
/usr/local/nginx/sbin/nginx

// 主配置文件目录（默认）
/usr/local/nginx/conf/nginx.conf
// 自己添加配置文件
/root/nginx/jingia.com/*.conf
```
###### 三、主配置文件内容(其中包含了自己添加的配置文件)，nginx.conf
```
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    include       /root/nginx/jingia.com/*.conf;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;

    server {
        listen       80;
        server_name  localhost;

        #charset koi8-r;

        #access_log  logs/host.access.log  main;

        location / {
            root   html;
            index  index.html index.htm;
        }

        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

        # proxy the PHP scripts to Apache listening on 127.0.0.1:80
        #
```
###### 四、自己添加的配置文件。注意http server 和https server 是写在一个server里的，分开写会有问题，下面的三个配置比较类似，后面两个没有配置https。（其中有使用到ssl证书，可以到腾讯云申请免费证书）
```
// www.conf
upstream www.jingia.com {
    server 127.0.0.1:3000;
    keepalive 8;
}

# the nginx server instance
server {
    listen 0.0.0.0:80;
    listen       443 ssl;
    server_name www.jingia.com;

    ssl_certificate      /root/ssl/1_www.jingia.com_bundle.crt;
    ssl_certificate_key  /root/ssl/2_www.jingia.com.key;

    ssl_session_cache    shared:SSL:1m;
    ssl_session_timeout  5m;

    ssl_ciphers  HIGH:!aNULL:!MD5;
    ssl_prefer_server_ciphers  on;

    location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;

      # value for proxy_pass has to match upstream name
      proxy_pass http://www.jingia.com/;
      proxy_redirect off;
    }
 }


// m.conf
upstream m.jingia.com {
    server 127.0.0.1:3001;
    keepalive 8;
}

# the nginx server instance
server {
    listen 0.0.0.0:80;
    server_name m.jingia.com;

    location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;

      # value for proxy_pass has to match upstream name
      proxy_pass http://m.jingia.com/;
      proxy_redirect off;
    }
 }


// all.conf
upstream jingia.com {
    server 127.0.0.1:3000;
    keepalive 8;
}

# the nginx server instance
server {
    listen 0.0.0.0:80;
    server_name jingia.com;

    location / {
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_set_header X-NginX-Proxy true;

      # value for proxy_pass has to match upstream name
      proxy_pass http://jingia.com/;
      proxy_redirect off;
    }
 }
```
###### 五、直接添加https配置，会报错，"ssl" parameter requires ngx_http_ssl_module，[参考文章](https://www.codelast.com/%E5%8E%9F%E5%88%9B-%E4%B8%BAnginx%E6%B7%BB%E5%8A%A0ssl%E6%94%AF%E6%8C%81%E6%A8%A1%E5%9D%97/)
- 1.查看旧的编译参数，（注意是大写的V，小写显示版本号）
```
/usr/local/nginx/sbin/nginx -V

// 得到--with-pcre=/root/lib/pcre-8.35 --with-zlib=/root/lib/zlib-1.2.11 --prefix=/usr/local/nginx
```
- 2.进入到源码目录，（我的是/root/lib/nginx-1.12.2），加上上面的编译参数，新加需要的ssl模块，重新编译
```
./configure --with-pcre=/root/lib/pcre-8.35 --with-zlib=/root/lib/zlib-1.2.11 --prefix=/usr/local/nginx --with-http_ssl_module

make
```
- 3.使用的是make，（不是make install，不然会被覆盖）
- 4.备份以前的nginx，替换需要的部分
```
cp /usr/local/nginx/sbin/nginx ~/bak/

cp objs/nginx /usr/local/nginx/sbin/
```
###### 六、./configure: error: SSL modules require the OpenSSL library，[参考文章](http://blog.csdn.net/testcs_dn/article/details/51461999)
```
yum -y install openssl openssl-devel
```
###### 七、暴力重启nginx命令
```
// 查询nginx主进程号
ps -ef | grep nginx

// 强制停止nginx
pkill -9 nginx

// 检查配置并启动
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
```
