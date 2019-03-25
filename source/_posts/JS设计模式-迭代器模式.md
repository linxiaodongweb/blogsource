---
title: JS设计模式-迭代器模式
date: 2019-03-16 14:27:21
tags: 
  - JavaScript
  - 前端
  - 设计模式
comment: true
---
##### 介绍
- 顺序访问一个集合
- 使用者无需知道集合的内部结构（封装）
迭代器模式通常都是对一个数组，集合等进行访问，迭代器的设计是为了封装一个方法，可以对多种数据类型进行访问。
<!--more-->
##### 代码说明
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>jquery each</p>
    <p>jquery each</p>
    <p>jquery each</p>

    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        var arr = [1, 2, 3]
        var nodeList = document.getElementsByTagName('p')
        var $p = $('p')

        // 要对这三个变量进行遍历，需要写三个遍历方法
        // 第一
        arr.forEach(function (item) {
            console.log(item)
        })
        // 第二
        var i, length = nodeList.length
        for (i = 0; i < length; i++) {
            console.log(nodeList[i])
        }
        // 第三
        $p.each(function (key, p) {
            console.log(key, p)
        })

        // 如何能写出一个方法来遍历这三个对象呢
        function each(data) {
            var $data = $(data)
            $data.each(function (key, p) {
                console.log(key, p)
            })
        }
        each(arr)
        each(nodeList)
        each($p)
    </script>
</body>
</html>
```
##### UML类图
![uml_iterator.png](https://upload-images.jianshu.io/upload_images/8878633-8c6d2ad0f21056f8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### UML类图代码演示
```javascript
class Iterator {
    constructor(conatiner) {
        this.list = conatiner.list
        this.index = 0
    }
    next() {
        if (this.hasNext()) {
            return this.list[this.index++]
        }
        return null
    }
    hasNext() {
        if (this.index >= this.list.length) {
            return false
        }
        return true
    }
}

class Container {
    constructor(list) {
        this.list = list
    }
    getIterator() {
        return new Iterator(this)
    }
}

// 测试代码
let container = new Container([1, 2, 3, 4, 5])
let iterator = container.getIterator()
while(iterator.hasNext()) {
    console.log(iterator.next())
}
```

##### 场景
- jQuery each
- ES6 Iterator
###### jQuery each
```javascript
     function each(data) {
            var $data = $(data)
            $data.each(function (key, p) {
                console.log(key, p)
            })
        }
        each(arr)
        each(nodeList)
        each($p)
```
###### ES6 Iterator
ES6 Iterator为何存在？
- ES6语法中，有序集合的数据类型已经有很多
- Array Map Set String TypedArray arguments NodeList Generator(慢慢被ES7 async await替代）
- 需要有一个统一的遍历接口来遍历多有数据类型
- （注意，object不是有序集合，可以用Map来代替)

ES6 Iterator是什么？
- 以上数据类型，都有[Sysbol.iterator]属性
- 属性值是函数，执行函数返回一个迭代器
- for...of的出现

具体教程可以看[阮一峰ES6](http://es6.ruanyifeng.com/#docs/iterator)
ES6 迭代器演示
```javascript
let arr = [1, 2, 3, 4]
let nodeList = document.getElementsByTagName('p')
let m = new Map()
m.set('a', 100)
m.set('b', 200)


function each(data) {
     生成遍历器
    let iterator = data[Symbol.iterator]()

    console.log(iterator.next())  // 有数据时返回 {value: 1, done: false}
    console.log(iterator.next())
    console.log(iterator.next())
    console.log(iterator.next())
    console.log(iterator.next())  // 没有数据时返回 {value: undefined, done: true}

    let item = {done: false}
    while (!item.done) {
        item = iterator.next()
        if (!item.done) {
            console.log(item.value)
        }
    }
each(arr)
each(nodeList)
each(m)

```
执行结果
![image.png](https://upload-images.jianshu.io/upload_images/8878633-4ebec7c307b45b11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```javascript
// Symbol.iterator 并不是人人都知道
// 也不是每个人都需要封装一个each方法
// 因此有了 `for...of`语法
// data需要包含Symbol.iterator这个属性才可以
function each(data) {
    for (let item of data) {
        console.log(item)
    }
}

each(arr)
each(nodeList)
each(m)
``
##### 设计原则验证
- 迭代器对象和目标对象分离
- 迭代器将使用者与目标对象隔离开
- 符合开放封闭原则

> 1. 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程。
