---
title: guidelines
date: 2018-05-03 12:03:56
tags:
  - 前端代码规范
---
# 前端代码规范
###### Front-End Coding Guidelines
- 以下规范是团队基本约定的内容，必须严格遵循 *20180503*

### CSS规范
##### 一、顺序
###### - 1.位置属性(position, top, right, z-index, display, float等)
###### - 2.大小(width, height, padding, margin)
###### - 3.文字系列(font, line-height, letter-spacing, color- text-align等)
###### - 4.背景(background, border等)
###### - 5.其他(animation, transition等)
##### 二、命名
###### - 1.长名称或词组使用中横线，如果更长，使用中横线和下划线组合，下划线后为从属
###### - 2.能良好区分JavaScript变量命名（JS变量命名是用“_”）
##### 三、缩写
###### - 1.缩写属性
```
.list-box {
  border-top: 0;
  font: 100%/1.2 serif;
  padding: 0 1em 2em;
}
```
###### - 2.去掉小数点前的“0”
```
.list-item {
  font-size: .8em;
}
```
###### - 3.十六进制颜色代码缩写
```
.list-item {
  color: #0ff;
}
```
### HTML规范
##### 一、声明
###### - 1.必须加上 DOCTYPE 声明，并统一使用 HTML5 的文档声明
```
<!DOCTYPE html>
```
###### - 2.推荐使用属性值 cmn-Hans-CN（简体, 中国大陆），但是考虑浏览器和操作系统的兼容性，目前仍然使用 zh-CN 属性值
```
<html lang="zh-CN">
```
###### - 3.一般情况下统一使用 “UTF-8” 编码
```
<meta charset="UTF-8">
```
###### - 4.约定空元素标签都不加 “/” 字符
```
<br>
```
###### - 5.元素属性使用双引号，属性值可以写上的都写上
```
<input type="text">
  
<input type="radio" name="name" checked="checked" >
```
###### - 6.纯数字输入框，使用 type="tel" 而不是 type="number"
```
<input type="tel">
```
###### - 7.代码嵌套，每个块状元素独立一行，内联元素可选
```
<div>
    <h1></h1>
    <p></p>
</div>  
<p><span></span><span></span></p>
```
###### - 8.段落元素与标题元素只能嵌套内联元素
```
<h1><span></span></h1>
<p><span></span><span></span></p>
```
###### - 9.缩进建议使用2个空格，和eslint标准规范一样
###### - 10.移动端meta
```
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, shrink-to-fit=no" >
<meta name="format-detection" content="telephone=no" >
```
###### - 11.单位rem设置（放置到head标签，不要放到body）
```
<script> ! function(a) {var j, b = a.document, c = b.documentElement, d = a.orientation || 0, e = 750, f = 9 / 16, g = 100, h = a.devicePixelRatio, i = 1; b.write('<meta name="viewport" content="initial-scale=' + i + ",maximum-scale=" + i + ",minimum-scale=" + i + ',user-scalable=0">'), j = c.getBoundingClientRect().width, 90 == Math.abs(d) && (j *= f), c.style.fontSize = j / h * g / (e / h) + "px"}(window); </script>
```
###### - 12.重置样式
```
/* 移动端 */
/**
 * Eric Meyer's Reset CSS v2.0 (http://meyerweb.com/eric/tools/css/reset/)
 * http://cssreset.com
 */
html,body,div,span,applet,object,iframe,h1,h2,h3,h4,h5,h6,p,blockquote,pre,a,abbr,acronym,address,big,cite,code,del,dfn,em,img,ins,kbd,q,s,samp,small,strike,strong,sub,sup,tt,var,b,u,i,center,dl,dt,dd,ol,ul,li,fieldset,form,label,legend,table,caption,tbody,tfoot,thead,tr,th,td,article,aside,canvas,details,embed,figure,figcaption,footer,header,menu,nav,output,ruby,section,summary,time,mark,audio,video,input{margin:0;padding:0;border:0;font-size:100%;font-weight:normal;vertical-align:baseline;}
/* HTML5 display-role reset for older browsers */
article,aside,details,figcaption,figure,footer,header,menu,nav,section{display:block;}
body{line-height:1;}
blockquote,q{quotes:none;}
blockquote:before,blockquote:after,q:before,q:after{content:none;}
table{border-collapse:collapse;border-spacing:0;}

/* PC端 */
html, body, div, h1, h2, h3, h4, h5, h6, p, dl, dt, dd, ol, ul, li, fieldset, form, label, input, legend, table, caption, tbody, tfoot, thead, tr, th, td, textarea, article, aside, audio, canvas, figure, footer, header, mark, menu, nav, section, time, video { margin: 0; padding: 0; }
h1, h2, h3, h4, h5, h6 { font-size: 100%; font-weight: normal }
article, aside, dialog, figure, footer, header, hgroup, nav, section, blockquote { display: block; }
ul, ol { list-style: none; }
img { border: 0 none; vertical-align: top; }
blockquote, q { quotes: none; }
blockquote:before, blockquote:after, q:before, q:after { content: none; }
table { border-collapse: collapse; border-spacing: 0; }
strong, em, i { font-style: normal; font-weight: normal; }
ins { text-decoration: underline; }
del { text-decoration: line-through; }
mark { background: none; }
input::-ms-clear { display: none !important; }
body { font: 12px/1.5 \5FAE\8F6F\96C5\9ED1, \5B8B\4F53, "Hiragino Sans GB", STHeiti, "WenQuanYi Micro Hei", "Droid Sans Fallback", SimSun, sans-serif; background: #fff; }
a { text-decoration: none; color: #333; }
a:hover { text-decoration: underline; }
```

More info: [Front-End Coding Guidelines @2017 凹凸实验室](https://guide.aotu.io/index.html)
