---
title: JS设计模式-其他2-组合模式 & 享元模式
date: 2019-03-25 09:49:35
tags:
  - JavaScript
  - 设计模式
---
### 组合模式
##### 概念
- 生成树形结构，表示 “整体-部分”
- 让整体和部分都具有一致的操作方式
例如文件夹目录
<!-- more -->
![image.png](https://upload-images.jianshu.io/upload_images/8878633-1cd0c0f87555fd5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
虚拟DOM中的vnode是这种形式，但数据类型简单。
##### 演示
```html
 <div id="div1" class="container">
        <p>123</p>
        <p>456</p>
 </div>
```
```javascript
 {
    tag: "div",
    attr: {
        id: "div1",
        className: "container"
    },
    children: [
       {
            tag: "p",
            attr: {},
            children: ["123"]
       },
       {
            tag: "p",
            attr: {},
            children: ["456"]
       },
    ]
}
```
##### 其他
我们常用的菜单一般也是组合模式的
##### 设计原则验证
- 将整体和单个节点的操作抽象出来
- 符合开放封闭原则
### 享元模式
##### 概念
- 共享内存（主要考虑内存，而非效率）
- 相同的数据，共享使用
前端的事件代理可以看成一个简单的例子
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <!-- 无限下拉列表，将事件代理到高层节点上 -->
    <!-- 如果都绑定到'<a>'标签，对内存开销太大，容易浏览器卡死 -->
    <div id="div1" class="container">
        <a href="#">a1</a>
        <a href="#">a2</a>
        <a href="#">a3</a>
        <a href="#">a4</a>
        <!-- ...无限下拉 -->
    </div>
</body>
<script>
    const div1 = document.querySelector("#div1");
    div1.addEventListener("click", (e) => {
        const target = e.target;
        if(target.nodeName === "A"){
            alert(target.innerHTML)
        }
    })
</script>
</html>
```
##### 设计原则验证
- 将相同的部分抽象出来（因为他们共享的数据一样，只用改一个地方）
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程