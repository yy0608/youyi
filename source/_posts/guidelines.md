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
###### - 11.重置样式
```
/* 移动端 */
* { -webkit-tap-highlight-color: transparent; outline: 0; margin: 0; padding: 0; vertical-align: baseline; }
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin: 0; padding: 0; vertical-align: baseline; }
img { border: 0 none; vertical-align: top; }
i, em { font-style: normal; }
ol, ul { list-style: none; }
input, select, button, h1, h2, h3, h4, h5, h6 { font-size: 100%; font-family: inherit; }
table { border-collapse: collapse; border-spacing: 0; }
a { text-decoration: none; color: #666; }
body { margin: 0 auto; min-width: 320px; max-width: 640px; height: 100%; font-size: 14px; fon-family: -apple-system,Helvetica,sans-serif; line-height: 1.5; color: #666; -webkit-text-size-adjust: 100% !important; text-size-adjust: 100% !important; }
input[type="text"], textarea { -webkit-appearance: none; -moz-appearance: none; appearance: none; }

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


