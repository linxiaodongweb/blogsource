---
title: JS设计模式-装饰器模式
date: 2019-03-07 23:23:21
tags: 
  - JavaScript
  - 设计模式
comment: true
---
##### 介绍
- 为对象添加新功能
- 不改变其原有的结构和功能
和适配器模式不一样，适配器模式是原有的不能用了，而装饰器模式是原来的还能用，不过给增加一些功能。
比如： 手机壳，用来给手机美观，保护，防滑等等
<!-- more -->
##### UML类图
![装饰器UML类图](https://upload-images.jianshu.io/upload_images/8878633-f22bb51e19571f66.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 代码演示
```javascript
class Circle {
    draw() {
        console.log("画一个圆形");
    }
}

class Decorator {
    constructor(circle){
        this.circle = circle;
    }
    draw() {
        this.circle.draw();
        this.setRedBorder(this.circle);
    }
    setRedBorder(circle) {
        console.log("设置红色边框")
    }
}
// 测试代码
let circle = new Circle();
circle.draw()

let dec = new Decorator(circle);
dec.setRedBorder();
```
##### 场景
- ES7装饰器-babel可转换
- core-decorators等第三方库常用装饰器
ES7装饰器
##### 装饰类
 一个简单的demo
```javascript
// 一个简单的demo
function testDec(target) {
    target.isDec = "林海"
}

@testDec
class Demo {}

alert(Demo.isDec)
```
装饰类的原理
```javascript
// 装饰器的原理
@decorator
class A {}

//等同于

class A {}
A = decorator(A) || A;
```
可以加参数
```javascript
function testDec(isDec) {
    return function(target){
        target.isDec = isDec;
    }
}

@testDec(true)
class Demo {}

alert(Demo.isDec)
```
我们来做一个常用的mixins混合装饰器，来把一个类里面属性和方法全部添加到另一个类上
```javascript
function mixins(...list) {
    return function (target) {
      Object.assign(target.prototype, ...list)
    }
  }
  
  const Foo = {
    foo() { alert('foo') }
  }
  
  @mixins(Foo)
  class MyClass {}
  
  let obj = new MyClass();
  obj.foo() // 'foo'
```
##### 装饰方法
让某个方法只读，不能修改
```javascript
function readonly(target, name, descriptor){
    // descriptor属性描述对象 （Object.defineProperty 会用到)
    // descriptor对象原来的值如下
    // {
    //   value: specifiedFunction,
    //   enumerable: false,
    //   configurable: true,
    //   writable: true
    // };
    descriptor.writable = false;
    return descriptor;
  }
  
  class Person {
      constructor() {
          this.first = 'A'
          this.last = 'B'
      }
  
      @readonly
      testname() { return `${this.first} ${this.last}` }
  }
  
  var p = new Person()
  console.log(p.testname())
  p.testname = function () {} // 这里会报错，因为 name 是只读属性
```
一个打印日志装饰器
```javascript
function log(target, name, descriptor) {
    var oldValue = descriptor.value;
    // name 是修饰的方法名字
    descriptor.value = function() {
      console.log(`Calling ${name} with`, arguments);
      return oldValue.apply(this, arguments);
    };
  
    return descriptor;
  }
  
  class Math {
    @log
    add(a, b) {
      return a + b;
    }
  }
  
  const math = new Math();
  const result = math.add(2, 4);
  console.log('result', result);
```
装饰类的时候，一般主要是看target（装饰对象）第一个参数。
装饰方法的时候，一般主要看的是descriptor（描述）第三个参数。
##### core-decorators 
- 第三方开源 lib
- 提供常用的装饰器,比如readonly只读状态,autobind绑定this等
- 查阅文档： https://github.com/jayphelps/core-decorators

```javascript
// 首先安装
npm install core-decorators --save
```
这里我们使用 core-decorators提供的deprecate(对装饰的方法在浏览器中提示已经废弃）

```javascript
import { deprecate } from 'core-decorators';

class Person {
  @deprecate
  facepalm() {}

  @deprecate('We stopped facepalming')
  facepalmHard() {}

  @deprecate('We stopped facepalming', { url: 'http://knowyourmeme.com/memes/facepalm' })
  facepalmHarder() {}
}

let person = new Person();

person.facepalm();
// DEPRECATION Person#facepalm: This function will be removed in future versions.

person.facepalmHard();
// DEPRECATION Person#facepalmHard: We stopped facepalming

person.facepalmHarder();
// DEPRECATION Person#facepalmHarder: We stopped facepalming
//
//     See http://knowyourmeme.com/memes/facepalm for more details.

```
运行结果如下图：
![运行结果](https://upload-images.jianshu.io/upload_images/8878633-48d0739e659f1151.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 设计原则验证
- 将现有对象和装饰器进行分离，两者独立存在
- 符合开放封闭原则
##### 对装饰器更多学习，可以看 [阮一峰es6装饰器教程](http://es6.ruanyifeng.com/#docs/decorator)

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程