---
title: JS设计模式-其他4-命令模式 & 备忘录模式
date: 2019-03-23 09:52:52
tags:
  
  - 前端
  - 设计模式
---
### 命令模式
##### 概念
- 执行命令时，发布者和执行者分开
- 中间加入命令对象，作为中转站
<!-- more -->
开店的时候，店里有三五个人，店长一吆喝都知道。但是店里如果有上千个人，
吆喝就不合适了，这时候需要有一个广播通知所有店员。
或者说抗战片，一个团长，直接发令士兵肯定都是不能全部听到的，团长发布命令，然后小号冲锋声吹响，通知所有士兵。
##### 图示
![image.png](https://upload-images.jianshu.io/upload_images/8878633-6085d609560432f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 代码演示
```javascript
// 接收者
class Receiver {
    exec() {
        console.log("执行");
    }
}

// 命令者
class Command {
    constructor(receiver){
        this.receiver = receiver;
    }
    // 触发
    cmd() {
        console.log("执行命令")
        this.receiver.exec();
    }
}
// 触发者
class Invoker {
    constructor(command){
        this.command = command;
    }
    // 触发
    invoke() {
        console.log("开始")
        this.command.cmd();
    }
}

// 测试
// 士兵
let solider = new Receiver();
// 小号手
let trumpeter = new Command(solider);
// 将军
let general = new Invoker(trumpeter);
general.invoke();
// 开始
// 执行命令
// 执行
```
##### JS中的应用
- 网页富文本编辑器操作，浏览器封装了一个命令对象
```javascript
// execCommand 浏览器封装富文本操作，复制，加粗选中文字等
document.execCommand("bold")
document.execCommand("undo")
```
##### 设计原则验证
- 命令对象于执行对象分开，解耦
- 符合开放封闭原则

### 备忘录模式
##### 概念
- 随时记录一个对象的状态变化
- 随时可以恢复之前的某个状态（如撤销功能）
- 常见工具（编辑器）

##### 代码演示
```javascript
// 状态备忘
class Memento {
    constructor(content){
        this.content = content;
    }
    getContent() {
        return this.content;
    }
}
// 备忘列表
class CareTaker {
    constructor(){
        this.list = []
    }
    add(memento){
        this.list.push(memento)
    }
    get(index){
        return this.list[index]
    }
}
// 编辑器
class Editor {
    constructor (){
        this.content = null;
    }
    setContent(content){
        this.content = content;
    }
    getContent(){
        return this.content;
    }
    // 保存
    saveContentToMemento(){
        return new Memento(this.content);
    }
    // 恢复
    getContentFromMemento(memento){
        this.content = memento.getContent();
    }
}
// 测试代码
let editor = new Editor();
let careTaker = new CareTaker();
editor.setContent("111");
editor.setContent('222')
careTaker.add(editor.saveContentToMemento());  // 存储备忘录
editor.setContent("333");
careTaker.add(editor.saveContentToMemento());
editor.setContent("444");

console.log(editor.getContent());
editor.getContentFromMemento(careTaker.get(1));
console.log(editor.getContent());
editor.getContentFromMemento(careTaker.get(0));
console.log(editor.getContent());
```
运行结果
![运行结果.png](https://upload-images.jianshu.io/upload_images/8878633-c7d7e517f91b5295.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### 设计原则验证
- 状态对象于使用者分开，解耦
- 符合开放封闭原则

> 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程

