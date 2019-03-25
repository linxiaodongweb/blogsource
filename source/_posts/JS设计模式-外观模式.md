---
title: JS设计模式-外观模式
date: 2019-03-12 22:56:10
tags:
  - JavaScript
  - 设计模式
  - 前端

---
##### 什么是外观模式
> 本段内容来自于 [JAdam博客](https://www.cnblogs.com/adamjwh/p/9048594.html)

  有些人可能炒过股票，但其实大部分人都不太懂，这种没有足够了解证券知识的情况下做股票是很容易亏钱的，刚开始炒股肯定都会想，如果有个懂行的帮帮手就好，其实基金就是个好帮手，支付宝里就有许多的基金，它将投资者分散的资金集中起来，交由专业的经理人进行管理，投资于股票、债券、外汇等领域，而基金投资的收益归持有者所有，管理机构收取一定比例的托管管理费用。

  <!-- more -->    
　　其实本篇要说的这个设计模式就和这很有关系，由于当投资者自己买股票时，由于众多投资者对众多股票的联系太多，反而不利于操作，这在软件中就成为耦合性太高，而有了基金后，就变成众多用户只和基金打交道，关心基金的上涨和下跌，而实际上的操作确是基金经理人与股票和其它投资产品打交道，这就是外观模式。

##### 介绍
- 为子系统中的一组接口提供了一个高级接口
- 使用者使用这个高级接口
看完后应该有一点印象，在看看这张图片就理解了
![外观模式.png](https://upload-images.jianshu.io/upload_images/8878633-27105ebc7718f35f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果没有这个高级接口，我们的代码链接可能很混乱难维护。
外观模式在结合业务的场景中很常用。
##### UML类图
![uml_facade.png](https://upload-images.jianshu.io/upload_images/8878633-a7ebd17441bd7ccf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
##### 场景
下面的列子就是一种简单的外观模式
```javascript
function bindEvent(elem, type, selector, fn) {
  if ( fn == null) {
    fn = selector
    selector = null
  }  
  // *****
}

//  调用
bindEvent(elem, 'click', '#div1', fn)
bindEvent(elem, 'click', fn)
```
##### 设计原则验证
不符合单一职责原则和开放封闭原则，因此要注意谨慎使用，不可滥用

> 1. 本文资料来自慕课网-双越老师-Javascript 设计模式系统讲解与应用视频课程。
