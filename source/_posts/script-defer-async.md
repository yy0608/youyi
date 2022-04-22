---
title: script标签的defer和async属性
tags:
  - script标签
abbrlink: 31089
date: 2022-04-22 14:47:56
---

###### 在不加任何异步属性的情况下，script的下载和执行都会阻塞dom的渲染，而添加异步属性可以使下载阶段异步进行
###### async属性：
- 1. 异步下载script代码
- 2. 不支持内联方式，也就是script标签必须有src属性
- 3. 执行时机：下载完后，立即执行（下载不阻塞进程）
###### defer属性: 
- 1. 异步下载script代码（同async）
- 2. 不支持内联方式，也就是script标签必须有src属性（同async）
- 3. 执行时机：下载完后，在dom解析完之后、触发DOMContentLoaded之前执行
- 4. 执行顺序：如果带defer的script有多个，那它们将按照在页面中出现的顺序来依次执行
###### 动态创建
- 1. 通过src属性赋值动态创建的script，未指定async属性时也是默认异步的
```
var script = document.createElement('script')
script.src = 'file.js'
document.body.appendChild(script)
```
- 2. 通过innerHtml或eval方式创建的script，未指定async属性时默认不是异步
```
document.body.innerHTML = '<script src="file.js"></script>
```
###### 总结
- 1. 如果脚本无需等待页面解析，且无依赖独立运行，那么应使用 async
- 2. 如果脚本需要等待页面解析，且依赖于其它脚本，调用这些脚本时应使用 defer，将关联的脚本按所需顺序置于 HTML 中
- 3. 文章开始时讲到任何情况下js的执行都会阻塞dom的渲染。同样的，async和defer也一样，它们只是使js的下载阶段异步，执行阶段仍然会阻塞dom
