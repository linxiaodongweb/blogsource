---
title: JS设计模式-工厂模式
date: 2019-03-02 10:36:44
tags: ["JavaScript"]
---

# 工厂模式介绍
##### 什么是工厂模式
工厂模式是我们最常用的实例化对象模式了，是用工厂方法代替new操作的一种模式。著名的Jive论坛 ,就大量使用了工厂模式，工厂模式在Java程序系统可以说是随处可见。因为工厂模式就相当于创建实例对象的new，我们经常要根据类Class生成实例对象，如A a=new A() 工厂模式也是用来创建实例对象的，所以以后new时就要多个心眼，是否可以考虑使用工厂模式，虽然这样做，可能多做一些工作，但会给你系统带来更大的可扩展性和尽量少的修改量。
属于三大设计类型中的创建型模式。
<!-- more -->
##### 什么时候用工厂模式，使用场景等？
-  将 new 操作单独封装
-  遇到 new 时，就要考虑是否该使用工厂模式。
##### 示例
- 你去购买汉堡，直接点餐，取餐，不用自己亲手做。
- 商店要 ”封装" 做汉堡的工作，做好直接给买者。

##### 工厂模式UML类图如下
![工厂模式UML类图](https://linxiaodongweb.github.io/images/uml-factory.jpeg)
其中Creator是一个工厂，Product 是一个产品。
一个Creator工厂有一个create方法，返回一个产品，可以理解为最简单的工厂模式。

# 代码演示
```
class Product {
    constructor(name) {
        this.name = name;
    }

    init() {
        alert("init");
    }
    
    fn1() {
        alert("fn1");
    }
    fn2() {
        alert("fn2");
    } 
}
class Creator {
    create(name){
        return new Product(name);
    }
}

// 测试
let creator = new Creator();
let p = creator.create("p1");
p.init();
p.fn1();

// 结果 弹出 init  111 
```
*结论： 我们通过Creator类提供的create方法来创建Product， 通过Creator工厂已经把真正的构造函数封装起来了，我们用的时候只需要知道Creator工厂的create方法可以产生一个实例，而不用去关心生成的实例是谁。*

##### 常见场景
- Jquery - $("div")
- React.createElement
- Vue 异步组件

1 Jquery工厂模式简单demo
```
class Jquery {
    constructor(selector){
        let slice = Array.prototype.slice;
        let dom = slice.call(document.querySelectorAll(selector));
        let len = dom ? dom.length : 0;
        for(let i = 0; i < len; i++) {
            this[i] = dom[i];
        }
        this.length = len;
        this.selector = selector || "";
    }
    append(node){

    }
    addClass(name){

    }
    html(data){

    }
}
window.$ = function (selector) {
    return new Jquery(selector);
}
```

2 React.createElement简单demo
```
class VNode (tag, attrs, children)  { // 此处写法便于理解，语法错误请忽略
   // 省略内部代码
}
React.createElement = function (tag, attrs, children) {
    return new Vnode(tag, attrs, children);
}
```
> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程