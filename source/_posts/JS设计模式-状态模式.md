---
title: JS设计模式-状态模式
date: 2019-03-17 21:54:59
tags:
---
##### 介绍
- 一个对象有状态变化
- 每次状态变化都会触发一个逻辑
- 不能总是使用if...else来控制
eg: 红绿灯  收藏/未收藏
核心： 状态和主体分离
  <!-- more -->    
##### UML类图
![uml_状态模式类图.png](https://upload-images.jianshu.io/upload_images/8878633-c93c9d1ec571c90e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 代码演示
```javascript
class State {
    constructor(color) {
        this.color = color
    }
    handle(context) {
        console.log(`turn to ${this.color} light`)
        context.setState(this)
    }
}

class Context {
    constructor() {
        this.state = null
    }
    setState(state) {
        this.state = state
    }
    getState() {
        return this.state
    }
}

// 测试代码
let context = new Context()

let greed = new State('greed')
let yellow = new State('yellow')
let red = new State('red')

// 绿灯亮了
greed.handle(context)
console.log(context.getState())
// 黄灯亮了
yellow.handle(context)
console.log(context.getState())
// 红灯亮了
red.handle(context)
console.log(context.getState())

```
##### 场景
- 有限状态机
- 写一个简单的Promise
###### 有限状态机 
- 有限个状态，以及在这些状态间的变化
- 如交通信号灯
- 使用开源lib :[javascript-state-machine](https://github.com/jakesgordon/javascript-state-machine)
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <p>有限状态机</p>
    <button id="btn"></button>
    
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <script src="./03-javascript-state-machine.js"></script>
    <script>
        // 状态机模型
        var fsm = new StateMachine({
            init: '收藏',  // 初始状态，待收藏
            transitions: [
                {
                    name: 'doStore', 
                    from: '收藏',
                    to: '取消收藏'
                },
                {
                    name: 'deleteStore',
                    from: '取消收藏',
                    to: '收藏'
                }
            ],
            methods: {
                // 执行收藏
                onDoStore: function () {
                    alert('收藏成功')
                    updateText()
                },
                // 取消收藏
                onDeleteStore: function () {
                    alert('已取消收藏')
                    updateText()
                }
            }
        })

        var $btn = $('#btn')

        // 点击事件
        $btn.click(function () {
            if (fsm.is('收藏')) {
                fsm.doStore(1)
            } else {
                fsm.deleteStore()
            }
        })

        // 更新文案
        function updateText() {
            $btn.text(fsm.state)
        }

        // 初始化文案
        updateText()
    </script>
</body>
</html>
```

###### 写一个Promise
Promise 三个状态
- pending 准备
- fullfilled 成功 （执行resolve)
- rejected 失败

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    
    <script src="https://cdn.bootcss.com/jquery/3.3.1/jquery.min.js"></script>
    <script src="./03-javascript-state-machine.js"></script>
    <script>
        // 模型
        var fsm = new StateMachine({
            init: 'pending',
            transitions: [
                {
                    name: 'resolve',
                    from: 'pending',
                    to: 'fullfilled'
                },
                {
                    name: 'reject',
                    from: 'pending',
                    to: 'rejected'
                }
            ],
            methods: {
                // 成功
                onResolve: function (state, data) {
                    // 参数：state - 当前状态示例; data - fsm.resolve(xxx) 执行时传递过来的参数
                    data.successList.forEach(fn => fn())
                },
                // 失败
                onReject: function (state, data) {
                    // 参数：state - 当前状态示例; data - fsm.reject(xxx) 执行时传递过来的参数
                    data.failList.forEach(fn => fn())
                }
            }
        })

        // 定义 Promise
        class MyPromise {
            constructor(fn) {
                this.successList = []
                this.failList = []

                fn(() => {
                    // resolve 函数
                    fsm.resolve(this)
                }, () => {
                    // reject 函数
                    fsm.reject(this)
                })
            }
            then(successFn, failFn) {
                this.successList.push(successFn)
                this.failList.push(failFn)
            }
        }

        // 测试代码
        function loadImg(src) {
            const promise = new MyPromise(function (resolve, reject) {
                var img = document.createElement('img')
                img.onload = function () {
                    resolve(img)
                }
                img.onerror = function () {
                    reject()
                }
                img.src = src
            })
            return promise
        }
        var src = 'http://www.imooc.com/static/img/index/logo_new.png'
        var result = loadImg(src)
        console.log(result)

        result.then(function (img) {
            console.log('success 1')
        }, function () {    
            console.log('failed 1')
        })
        result.then(function (img) {
            console.log('success 2')
        }, function () {    
            console.log('failed 2')
        })

    </script>
</body>
</html>
```
##### 设计原则验证
- 将状态对象和主题对象分离，状态的变化逻辑单独处理
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程