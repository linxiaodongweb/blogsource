---
title: JS设计模式-单列模式
date: 2019-03-02 09:44:02
tags: 
  - JavaScript
  - 设计模式

comment: true
---

##### 什么是单例设计模式
- 系统中被唯一使用
- 一个类中只有一个实例
- 属于三大设计类型中的创建型模式  

  <!-- more -->    

##### UML 类图
<img src="https://upload-images.jianshu.io/upload_images/8878633-9b942598a6e181b7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240" width = "400" height = "490"/>

##### 示例
- 登录框
- 购物车

 说明
- 单例模式需要用到Java的特性 (private)
- ES6中没有（ typeScript除外）
- 只能用Java代码来演示 UML 图中的内容 （不要担心，JS中也会变相的实现单例模式）
Java代码演示 (Java代码是最容易理解的编程语言之一）
```javascript
public class SingleObject {
     // 注意，私有化构造函数，外部不能 new,  只能内部 new！！！！
     private SingleObejct(){
     }
     // 唯一被 new 出来的对象
     private SingleObject instance = null;
     // 获取对象的唯一接口
     public static SingleObject getInstance() {
           if(instance == null){
                 // 只 new一次
                 instance = new SingleObject();
           } 
           return instance;
     }
     // 对象方法
     public void login(username, password) {
            System.out.println("login...");
     }
}
```
```javascript
public class  SingletonPatternDemo {
      public static void main(String[] args) {
            // 不合适的构造函数
            //  编译时错误， 构造函数 SingleObject 是不可见的。
            //   SingleObject object = new SingleObject();

            // 获取唯一可用的对象
            SingleObject object = SingleObject.getInstance();
            object.login();
      }
}
```

##### JS中使用单例模式
```javascript
class SingleObject {
  login() {
    console.log("login..."):
  }
}

SingleObject.getInstance = (function () {
  let instance;
  return function () {
    if(!instance) {
      instance = new SingleObject();
    }
    return instance;
  }
})()
// 测试，注意这里只能使用静态函数 getInstance, 不能 new SingleObject() !!!
let obj1 = SingleObject.getInstance()
obj1.login();
let obj2 = SingleObject.getInstance()
obj2.login();
console.log(obj1 === obj2)  // true  两者必须完全相等

 // 区分
let obj3 = new SingleObejct();
console.log(obj1 === obj3);  // false 这里就不是单例模式了。
```

##### 场景
Jquery 只有一个$
模拟登录框
其他

Jquery $ Demo 代码可能不一样，但是思路是一样的。
```javascript
if (window.Jquery !== null ){
  return window.Jquery;
}else{
  // 初始化....   
}
```
模拟登录框
```javascript
class LoginForm {
    constructor() {
        this.state = 'hide'
    }
    show() {
        if (this.state === 'show') {
            alert('已经显示')
            return
        }
        this.state = 'show'
        console.log('登录框已显示')
    }
    hide() {
        if (this.state === 'hide') {
            alert('已经隐藏')
            return
        }
        this.state = 'hide'
        console.log('登录框已隐藏')
    }
}

LoginForm.getInstance = (function () {
    let instance
    return function () {
        if (!instance) {
            instance = new LoginForm();
        }
        return instance
    }
})()

// 一个页面中调用登录框
let login1 = LoginForm.getInstance()
login1.show()
// login1.hide()

// 另一个页面中调用登录框
let login2 = LoginForm.getInstance()
login2.show()

// 两者是否相等
console.log('login1 === login2', login1 === login2)
```
其他
购物车（和登录框类似）
vuex 和 redux 中的 store

##### 设计原则验证 （五大设计原则SOLID）
- 符合单一职责原则，只实例化唯一的对象。
- 没法具体体验开放封闭原则，但是绝对不违反开放封闭原则。

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程