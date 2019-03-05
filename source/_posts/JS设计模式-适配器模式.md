---
title: JS设计模式-适配器模式
date: 2019-03-05 22:44:43
tags:
    - JavaScript
---
##### 什么是适配器模式
- 旧接口模式和使用者不兼容
- 中间加一个适配器转接口
比如你去香港或者出国去一些国家，他们的插排接口和我们都不一样，我要充电不能直接用，这个时候就需要一个适配器来转换一下电压。
<!--more-->
##### UML类图
<img src="/images/uml-adaptee.jpeg" width = "630" height = "330"/>
##### 代码演示
```javascript
class Adaptee {
  specificRequest() {
      return "德国标准插头";
  }
}

class Target {
  constructor() {
    this.adaptee = new Adaptee()
  }
  request() {
    let info = this.adaptee.specificRequest();
    return `${info} - 转换器 - 中国插头`  
  }
}
// 测试
let target = new Target();
let res = target.request();
console.log(res);
```
##### 场景
- 封装旧接口
- vue computed
封装旧接口
```javascript
// 自己封装的 ajax 使用方法如下
ajax({
  url: '/getData',
  type: 'post',
  dataType: 'json',
  data: {
      id: "123"
  }
})
.done(function(){})
// 但因为历史原因，代码中全都是：
// $.ajax({...})
```
如果这时候不适用jquery了，手动去一个个替换$.ajax不靠谱，旧接口不兼容，这时候我们只需要一个适配器来封装旧接口来实现，而且以后的也还可以继续像Jquery使用
```javascript
// 做一层适配器
var $ = {
  ajax: function(options){
    return ajax(options);
  }
}
```
vue的计算属性也是一个适配器，这里我拿官网demo来展示下，可以仔细品味下
```html
<div id="app">
  <p>原始字符串: {{ message }}</p>
  <p>计算后反转字符串: {{ reversedMessage }}</p>
</div>
 
<script>
var vm = new Vue({
  el: '#app',
  data: {
    message: 'Runoob!'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
</script>
```
##### 设计原则验证
- 将旧接口和使用者进行分离
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程