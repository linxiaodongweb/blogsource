---
title: JS设计模式-其他3-策略模式 & 模板方法模式 & 职责链模式
date: 2019-03-21 09:50:46
tags:
  - JavaScript
  - 设计模式
---
### 策略模式
##### 概念
- 不同策略分开处理
- 避免出现大量 if...else 或者 switch...case
<!-- more -->

##### 演示
未使用策略模式的时候代码是这个样子的
```javascript
class User {
    constructor (type) {
        this.type = type;
    }
    buy() {
        if(this.type === "ordinary") {
            console.log("普通用户购买");
        } else if(this.type === "member") {
            console.log("会员用户购买");
        }else if(this.type === "VIP") {
            console.log("VIP用户购买");
        }
    }
}
// 测试代码
let u1 = new User('ordinary')
u1.buy()
let u2 = new User('member')
u1.buy()
let u3 = new User('VIP')
u1.buy()
```
使用策略模式的代码
```javascript
class OridnaryUser {
    buy() {
        console.log("普通用户购买")
    }
}
class MemberUser {
    buy() {
        console.log("会员用户购买")
    }
}
class VipUser {
    buy() {
        console.log("VIP用户购买")
    }
}
let u1 = new OridnaryUser();
let u2 = new MemberUser();
let u3 = new VipUser();
```

##### 设计原则验证
- 不同策略，分开处理，而不是混合在一起
- 符合开放封闭原则

### 模板方法模式
##### 什么是模板方法？
一个方法代码太多，我们可以拆分开来，或者组合其他的方法一起执行
可以通过一个方法，对自己内部的一些相关顺序执行方法进行一个合并，对外输出一个统一的方法。  
简单用代码演示
```javascript
class Action {
    handle() {
        handle1();
        handle2();
        handle3();
    }
    handle1() {
        console.log(1)
    }
    handle2() {
        console.log(2)
    }
    handle3() {
        console.log(3)
    }
}
```

### 职责链模式
##### 概念
- 异步操作可能分为多个职责角色来完成
- 把这些角色都分开，然后用一个链串起来
- 将发起者和各个处理者进行隔离
##### 代码演示
比如你需要请假，需要各个审批，一级一级的审批，代码如下
```javascript
class Action {
    constructor(name) {
        this.name = name;
        this.nextAction = null;
    }
    setNextAction(action) {
        this.nextAction = action;
    }
    handle() {
        console.log(`${this.name} 审批`)
        if(this.nextAction !== null){
            this.nextAction.handle();
        }
    }
}
// 测试
let a1 = new Action("组长");
let a2 = new Action("经理");
let a3 = new Action("总监");
a1.setNextAction(a2);
a2.setNextAction(a3);
a1.handle();
```
运行结果
![运行结果.png](https://upload-images.jianshu.io/upload_images/8878633-06d55d2f3ea390a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### JS中的链式操作
- 职责链模式和业务结合较多，JS中能够联想到链式操作
- Jquery的链式操作， Promise.then的链式操作

##### 设计原则验证
- 发起者于各个处理者进行隔离
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程
