---
title: JS设计模式-其他1-原型模式 & 桥接模式
date: 2019-03-17 22:35:51
tags:
  
  - 前端
  - 设计模式
---
其他设计模式系列开始介绍前端不常用的设计模式
有哪些设计模式？
1. 创建性模式
   原型模式
2. 结构型模式
   桥接模式  组合模式  享元模式
3. 行为型模式
   策略模式 模板方法模式 职责链模式 命令模式 备忘录模式 中介者模式 访问者模式 解释器模式
<!-- more -->
### 原型模式
##### 概念
clone自己，生成一个新对象
java默认有clone接口，不用自己实现。
##### JS中的应用 Object.create()
```javascript
// 一个原型 对象
let prototype = {
    getName: function () {
        return this.first + " " + this.last;
    },
    say: function () {
        alert("hello")
    }
}
// 基于原型创建x
let x = Object.create(prototype);
x.first = "A";
x.last = "B";
alert(x.getName());
x.say();
// 基于原型创建y
let y = Object.create(prototype);
y.first = "C";
y.last = "D";
alert(y.getName());
y.say();
```
##### 对比JS中的原型prototype
- prototype 可以理解为ES6 class 的一种底层原理实现
- 而class是实现面向对象的基础，并不是服务于某个模式
- 若干年后ES6全面普及，大家可能会忽略掉prototype
- 但是Object.create却会长久存在

### 桥接模式
##### 概念
- 用于把抽象化与实现化解耦
- 使得二中可以独立变化
-（未找到JS中的经典应用）
##### demo 
比如我们要画这样一幅图
![image.png](https://upload-images.jianshu.io/upload_images/8878633-348db8e67e6d3e59.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
黄色圆圈  红色圆圈  黄色三角 红色三角 这样四个图形
代码实现如果混合在一起一般是这个样子的
```javascript
class ColorShape {
    yellowCircle() {
        console.log("yellow circle")
    }
    redCircle() {
        console.log("red circle")
    }
    yellowTriangle() {
        console.log("yellow triangle")
    }
    redTriangle() {
        console.log("red triangle")
    }
}
// 测试
let cs = new ColorShape();
cs.yellowCircle()
cs.redCircle()
cs.yellowTriangle()
cs.redTriangle()
```
但是从前端模式来说，我们一般是分离开的，大概是这样的
![image.png](https://upload-images.jianshu.io/upload_images/8878633-647a6d9b8d406fe8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
画图是画图，填充颜色是填充颜色，比较适合一些复杂性比较大的业务。
代码实现是这个样子的
```javascript
class Color {
    constructor(name){
        this.name = name;
    }
}
class Shape {
    constructor(name, color){
        this.name = name;
        this.color = color;
    }
    draw(){
        console.log(`${this.color.name} ${this.name}`)
    }
}
// 测试代码
let red = new Color("red");
let yellow = new Color("yellow");
let circle = new Shape("circle", red);
circle.draw();
let triangle = new Shape("triangle", yellow);
triangle.draw();
```
##### 设计原则验证
- 抽象和实现分离
- 符合开放封闭原则


> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程