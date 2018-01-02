---
title: violentmonkey
date: 2018-01-02 16:58:42
tags:
  - violentmonkey
  - 暴力猴
---
###### 暴力猴violentmonkey是一个可以在浏览器插入js脚本的chrome插件，能插入脚本能做的其他事情就不用多说了。本文主要讲自己写脚本，如果下载其他百度云下载或者屏蔽视频广告可以自行搜索。
- 1.下载。chrome网上商店[下载](https://chrome.google.com/webstore/detail/violentmonkey/jinjaccalgkegednnccohejagnlnfdag?hl=zh-CN)
- 2.找到插件并新建脚本
- 3.可以尝试书写js保存，浏览器打开的每个页面都会弹出
```
alert('test')
```
- 4.match参数可以匹配需要的网页域名，也可以通过location.origin匹配
- 5.异步脚本插入函数，jquery插入完成之后进行其他操作
```
function appendJquery (url, callback){
  var script = document.createElement("script");
  script.type = "text/javascript";
  if (script.readyState){
    script.onreadystatechange = function () { // ie
        if (script.readyState == "loaded" || script.readyState == "complete") {
          script.onreadystatechange = null;
          callback();
        }
      };
    } else {
      //Others: Firefox, Safari, Chrome, and Opera
      script.onload = function(){callback();
    };
  }
  script.src = url;
  document.body.appendChild(script);
}
```
- 6.css插入函数，此处有es6的模板字符串，广告的隐藏很好用，常用在youtube上
```
function appendStyle () {
  let styleEle = document.createElement('style')
  styleEle.innerHTML = `
    #__bs_notify__ {
      display: none !important;
    }
  `
  document.body.appendChild(styleEle)
}
```
###### 以前有个抓取珍某网美女帅哥高清图的需求，暴力猴是主要功臣。思路是定时点击每个美女item，跳到详情页后保存高清图地址到localstorage，有分类的需求需要字符串拼接，拿到所有地址后，通过node解析字符串下载保存图片。这个是纯浏览器客户端的操作，应该是可以通过身份模拟操作的，但我还是更喜欢客户端可视化的操作。之前没有做各个页面间的通信，是固定的时间间隔打开页面。如果优化的话，可以通过监听storage事件，这个方法有局限性。在亚马逊商家前后台页面通信时，有实践过websocket，当然ws强大很多。
