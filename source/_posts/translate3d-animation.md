---
title: translate3d_animation
date: 2018-01-16 14:29:25
tags:
  - translate3d
---
###### translate3d配合perspective属性效果更佳。注意：
- 1.写在外层相对定位容器上
- 2.这个高度是目标高度的2倍
- 3.不要再外层相对定位上写overflow hidden，可以再加一个外层容器
```
-webkit-perspective: 696px;
perspective: 696px;
```
###### 判断鼠标进入容器的方向
```
function hoverDir(e, dom) {
  var w = dom.offsetWidth,
    h = dom.offsetHeight,
    x = (e.clientX - dom.offsetLeft - (w / 2)) * (w > h ? (h / w) : 1),
    y = (e.clientY - dom.offsetTop - (h / 2)) * (h > w ? (w / h) : 1),
    // 上(0) 右(1) 下(2) 左(3)
    direction = Math.round((((Math.atan2(y, x) * (180 / Math.PI)) + 180) / 90) + 3) % 4,
    eventType = e.type,
    dirName = new Array('top', 'right', 'bottom', 'left');
  if (e.type == 'mouseover' || e.type == 'mouseenter') { // 进入
    console.log(dirName[direction])
  } else { // 移出
    console.log(dirName[direction])
  }
}
```