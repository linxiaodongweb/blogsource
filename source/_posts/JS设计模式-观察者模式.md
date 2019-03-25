---
title: JS设计模式-观察者模式
date: 2019-03-12 23:11:01
tags:
  - JavaScript
  - 前端
  - 设计模式
---
观察者模式是前端设计模式的核心
##### 介绍
- 发布 & 订阅
- 一对多 
什么是发布 & 订阅？
  <!-- more -->    
我说好了一键事情，等着别人来做。  比如，我躺在家里，订了一份外卖，然后等着，会有人来给你触发。 
一对多就像一个拍卖品一样，同时可以有多个人观察它。

##### UML类图
![uml_observer.png](https://upload-images.jianshu.io/upload_images/8878633-d573d1c60a244f37.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
左侧是Observer，就是观察者，它有一个update方法，当观察者需要被触发的时候执行update。
右侧是主题，主题可以绑定多个观察者，放在observers里面。
主题可以获取状态（getState()),也可以设置状态（setState())
当状态设置完成后，他会触发所有的观察者（notifyAllObservers()),
触发所有观察者里面的update方法。
观察者定义好以后，它就等待被更新，等待被触发，当然，前提是它已经定义好了。
注意一对多，主题是一个，但是观察者可能是多个。
可能还不是很理解，可以看看下面代码演示，仔细看几遍，比较容易理解。
##### 代码演示
```javascript
// 主题，接收状态变化，触发每个观察者
class Subject {
  constructor() {
      this.state = 0
      this.observers = []
  }
  getState() {
      return this.state
  }
  setState(state) {
      this.state = state
      this.notifyAllObservers()
  }
  attach(observer) {
      this.observers.push(observer)
  }
  notifyAllObservers() {
      this.observers.forEach(observer => {
          observer.update()
      })
  }
}

// 观察者，等待被触发
class Observer {
  constructor(name, subject) {
      this.name = name
      this.subject = subject
      this.subject.attach(this)
  }
  update() {
      console.log(`${this.name} update, state: ${this.subject.getState()}`)
  }
}

// 测试代码
let s = new Subject()
let o1 = new Observer('o1', s)
let o2 = new Observer('o2', s)
let o3 = new Observer('o3', s)

s.setState(1)
s.setState(2)
s.setState(3)

```
运行结果如下图
![image.png](https://upload-images.jianshu.io/upload_images/8878633-5c754fe74dfcada7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### 场景
- 网页事件绑定
- Promise
- jQuery callbacks
- nodejs 自定义事件

###### 网页事件绑定
```html
<button id="btn1">btn</button>
<script>
   $('#app').click(function(){
    console.log(11);
   })
   $('#app').click(function(){
    console.log(22);
   })
   $('#app').click(function(){
    console.log(33);
   })
</script>
//  每次点击都会打印 11 22 33
```
##### Promise
```javascript
var src = 'http://www.imooc.com/static/img/index/logo_new.png'
var result = loadImg(src)
console.log(result)

result.then(function (img) {
  console.log('success 1')
}, function () {    
  console.log('failed 1')
})
// then就是一个观察者，等待前一个处理完成
```
##### Jquery $.Callbacks()
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>jQuery callbacks</p>    

    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.js"></script>
    <script>
        // 自定义事件，自定义回调
        var callbacks = $.Callbacks() // 注意大小写
        callbacks.add(function (info) {
            console.log('fn1', info)
        })
        callbacks.add(function (info) {
            console.log('fn2', info)
        })
        callbacks.add(function (info) {
            console.log('fn3', info)
        })
        callbacks.fire('gogogo')
        callbacks.fire('fire')
    </script>
</body>
</html>
```
##### nodejs 自定义事件
```javascript
const EventEmitter = require('events').EventEmitter

const emitter1 = new EventEmitter()
emitter1.on('some', () => {
    // 监听 some 事件
    console.log('some event is occured 1')
})
emitter1.on('some', () => {
    // 监听 some 事件
    console.log('some event is occured 2')
})
// 触发 some 事件
emitter1.emit('some')
```
继承
```javascript
const EventEmitter = require('events').EventEmitter

// 任何构造函数都可以继承 EventEmitter 的方法 on emit
class Dog extends EventEmitter {
    constructor(name) {
        super()
        this.name = name
    }
}
var simon = new Dog('simon')
simon.on('bark', function () {
    console.log(this.name, ' barked')
})
setInterval(() => {
    simon.emit('bark')
}, 500)
```
```javascript
var fs = require('fs')
var readStream = fs.createReadStream('./data/file1.txt')  // 读取文件的 Stream

var length = 0
readStream.on('data', function (chunk) {
    length += chunk.toString().length
})
readStream.on('end', function () {
    console.log(length)
})
```
```javascript
const EventEmitter = require('events').EventEmitter

// 任何构造函数都可以继承 EventEmitter 的方法 on emit
class Dog extends EventEmitter {
    constructor(name) {
        super()
        this.name = name
    }
}
var simon = new Dog('simon')
simon.on('bark', function () {
    console.log(this.name, ' barked')
})
setInterval(() => {
    simon.emit('bark')
}, 500)
```
```javascript
var fs = require('fs')
var readStream = fs.createReadStream('./data/file1.txt')  // 读取文件的 Stream

var length = 0
readStream.on('data', function (chunk) {
    length += chunk.toString().length
})
readStream.on('end', function () {
    console.log(length)
})
```
```javascript
var readline = require('readline');
var fs = require('fs')

var rl = readline.createInterface({
    input: fs.createReadStream('./data/file1.txt')
});

var lineNum = 0
rl.on('line', function(line){
    lineNum++
});
rl.on('close', function() {
    console.log('lineNum', lineNum)
});
```
##### 其他场景
- nodejs中：处理http请求，多进程通讯
- vue和react组件生命周期触发
- vue watch

##### nodejs 处理http请求
```javascript
var http = require('http')

function serverCallback(req, res) {
    var method = req.method.toLowerCase() // 获取请求的方法
    if (method === 'get') {
    }
    if (method === 'post') {
        // 接收 post 请求的内容
        var data = ''
        req.on('data', function (chunk) {
            // “一点一点”接收内容
            console.log('chunk', chunk.toString())
            data += chunk.toString()
        })
        req.on('end', function () {
            // 接收完毕，将内容输出
            console.log('end')
            res.writeHead(200, {'Content-type': 'text/html'})
            res.write(data)
            res.end()
        })
    }
    
}
http.createServer(serverCallback).listen(8081)  // 注意端口别和其他 server 的冲突
console.log('监听 8081 端口……')
```
##### 设计原则验证
- 主题和观察者分离，不是主动触发而是被动监听，两者解耦
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程
