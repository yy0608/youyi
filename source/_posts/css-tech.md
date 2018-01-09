---
title: CSS中的一些黑科技技巧
date: 2018-01-05 09:14:15
tags:
  - css3
---
- 1.鼠标移进去不见
```
.test {
  cursor: none;
}
```
- 2.文字模糊
```
.test {
  color: transparent;
  text-shadow: #111 0 0 5px;
}
```
- 3.多重边框
```
.test {
  box-shadow: 0 0 0 6px rgba(0, 0, 0, 0.2), 0 0 0 12px rgba(0, 0, 0, 0.2), 0 0 0 18px rgba(0, 0, 0, 0.2), 0 0 0 24px rgba(0, 0, 0, 0.2);
  height: 200px;
  margin: 50px auto;
  width: 400px;
}
```
- 4.css运算calc， 可用 + - * /，使用时注意加空格
```
.test {
  width: calc(100% - 20px);
}
```
- 5.border-radius高级使用
```
.test {
  border-radius: 10px;
}

.test { /* 左，右，下右，下左 */
  border-radius: 10px 20px 30px 40px;
}

.test { /* 斜线前面的影响的是水平方向，斜线后面影响的是垂直方向 */
  border-radius: 5px 5px 3px 2px / 5px 5px 1px 3px;
}
```