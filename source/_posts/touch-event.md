---
title: 移动端touch事件遇到的问题
date: 2018-01-08 16:10:16
tags:
  - touch事件
---
###### touchmove事件在部分安卓手机上只执行一次的问题
> 主要是由于200ms超时导致内核不一定会一直处理touchmove事件，一旦超时会将后续所有的事件转交给UI处理，导致touchmove不会一直触发。


```
e.preventDefault()
```

###### touchmove添加e.preventDefault()，影响页面默认滚动
> 通过判断是垂直滚动还是水平滚动，然后添加组织默认事件