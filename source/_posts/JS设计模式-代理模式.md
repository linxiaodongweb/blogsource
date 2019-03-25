---
title: JS设计模式-代理模式
date: 2019-03-09 21:40:13
tags:
  
  - 设计模式
  - 前端
---
##### 介绍
- 使用者无权访问目标对象
- 中间加代理，通过代理做授权和控制
比如说公司的内网，当我们在家的时候也是需要一个代理才能连接访问内网的信息。
##### 示例
- 科学上网，访问 github.com
- 明星的经纪人  （大多数情况下咱们不可能直接联系明星）
  <!-- more -->    
##### UML类图
![UML类图-代理模式](https://upload-images.jianshu.io/upload_images/8878633-e229c6fb056d93ad.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
代码说明
```javascript
class ReadImg {
  constructor(filename) {
    this.filename = filename;
    this.loadFromDisk() //初始化即从硬盘中加载， 模拟
  }
  display() {
    console.log('display... ' + this.filename);
  }
  loadFromDisk() {
    console.log('loading... ' + this.filename);
  }
}

class ProxyImg {
  constructor(filename){
    this.realImg = new ReadImg(filename);
  }
  display() {
    this.realImg.display();
  }
}

// test
let proxyImg = new ProxyImg("1.png");
proxyImg.display();
```
##### 场景
- 网页事件代理(事件委托）
- Jquery $.proxy
- ES6 Proxy
###### 事件委托代码实现
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>前端设计模式 - 代理模式</title>
</head>
<body>
    <div id="app">
        <p>aaa</p>
        <p>bbb</p>
        <p>ccc</p>
        <p>ddd</p>
    </div>
</body>
<script>
    const appDom = document.querySelector("#app");
    appDom.addEventListener("click", (e)=>{
        const target = e.target;
        if(target.nodeName="P"){
            console.log(target,target.innerHTML)
        }
    })
</script>
</html>
```
###### $.proxy 
本示例不考虑es6箭头函数解决this指向问题，假设我们维护的还是一个用Jquery的老项目。
```javascript
//正常的this使用
$('#myElement').click(function() {
    // 这个this是我们所期望的，当前元素的this.
    $(this).addClass('aNewClass');
});


//并非所期望的this
$('#myElement').click(function() {
    setTimeout(function() {
          // 这个this指向的是settimeout函数内部，而非之前的html元素
        $(this).addClass('aNewClass');
    }, 1000);
});
```
这时候通常做法是这样的
```javascript
$('#myElement').click(function() {
    var that = this;   //设置一个变量，指向这个需要的this
    setTimeout(function() {
          // 这个this指向的是settimeout函数内部，而非之前的html元素
        $(that).addClass('aNewClass');
    }, 1000);
});
```
但是，在使用了jquery框架的情况下， 有一种更好的方式，就是使用$.proxy函数。

jQuery.proxy(),接受一个函数，然后返回一个新函数，并且这个新函数始终保持了特定的上下文(context )语境。
上面的例子使用这种方式就可以修改成:
```javascript
$('#myElement').click(function() {
    setTimeout($.proxy(function() {
        $(this).addClass('aNewClass');  
    }, this), 1000);
});
```

###### 明星经纪人
```javascript
// 明星
let star = {
    // name和age可以返回，点化就需要经纪人代理给拦截了。
    name: '张XX',
    age: 25,
    phone: '13910733521'
}

// 经纪人
let agent = new Proxy(star, {
    get: function (target, key) {
        if (key === 'phone') {
            // 返回经纪人自己的手机号
            return '18611112222'
        }
        if (key === 'price') {
            // 明星不报价，经纪人报价
            return 120000
        }
        return target[key]
    },
    set: function (target, key, val) {
        if (key === 'customPrice') {
            if (val < 100000) {
                // 最低 10w
                throw new Error('价格太低')
            } else {
                target[key] = val
                return true
            }
        }
    }
})

// 主办方去找明星的经纪人
console.log(agent.name)  // 经纪人返回明星的姓名
console.log(agent.age)  // 明星的姓名
console.log(agent.phone) // 经纪人的点化，经纪人拦截了phone。
console.log(agent.price)  // 明星搞艺术，不谈钱，经纪人谈钱。

// 想自己提供报价（砍价，或者高价争抢）
agent.customPrice = 150000
// agent.customPrice = 90000  // 报错：价格太低
console.log('customPrice', agent.customPrice)
```
##### 设计原则验证
- 代理类和目标类分离，隔离开目标类和使用者
- 符合开放封闭原则
##### 代理模式 VS 适配器模式
- 适配器模式： 提供一个不同的接口（比如不同版本的插座插口)
- 代理模式： 提供一模一样的接口
##### 代理模式 VS 装饰器模式
- 装饰器模式： 扩展功能，原有功能不变且可直接使用，扩展功能不冲突原有功能。
- 代理模式： 显示原有功能，但是经过限制或者阉割之后的

> 1. 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程。
> 2. $.proxy 代码部分来自于[浪漫小生博客](https://www.cnblogs.com/hongchenok/p/3919497.html)