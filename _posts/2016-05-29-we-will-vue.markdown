---
layout:     post
title:      "We will vue! (零)"
subtitle:   "We will vue!"
date:       2016-05-29
author:     "J3n5en"
header-img: "http://ww4.sinaimg.cn/large/6bf00bd8jw1f4cl7i6uwdj20k00akjs6.jpg"
tags:
    - Javascripe
    - Front-end
    - Vue
---

为了引导同伴学习Vue.Js写下这系列文章。

在学习新东西的时候我一般要考虑`WWW`

- `Why`: 为什么要学习这个新技术，是遇到了什么困难？还是它能为你带来什么？
- `What`:要学的是什么？简介、介绍？
- `Where`:在哪里可以学习到这个东西，怎么学习

---

##　一、WHY ——浅谈前后端分离

> 合久必分，分久必合！

### 什么是前后端分离？

每个团队、每个人都有可能对前后端分离理解不一样。这里谈谈我的理解：

- 前端：负责View和Controller层。
- 后端：只负责Model层，业务处理/数据等。

![](http://ww4.sinaimg.cn/large/6bf00bd8jw1f4df3dyyocj20e809rq34.jpg)

> - 视图（View）：用户界面。
> - 控制器（Controller）：业务逻辑
> - 模型（Model）：数据保存

也就是说浏览器一端的显示、交互、逻辑处理是前端；前端需要获取数据、持久化数据、通知其他系统，这些无法在浏览器中单独完成，需要后端提供服务。

### 为什么要前后端分离？

传统的MVC架构决定了前端只能依赖后端。所以我们的开发模式依然是，前端写好静态demo，后端翻译成VM模版。再者就是在日后的维护中，代码往往前后端混杂在一起，毫无美感，根本没法维护。这里我就不吐槽了，估计你们也体验过那种'快感'。

### 怎么实现前后端分离？

这么系列叫做《We will Vue！》很明显，我是希望你们和我一样利用`Vue.js`这个MVVM库实现前后端分离，让前端脱离后端进行开发。

![MVC](http://ww1.sinaimg.cn/large/6bf00bd8jw1f4dfeahvd2j20g70e6mxm.jpg)

![MVVM](http://ww2.sinaimg.cn/large/6bf00bd8jw1f4dfd9w7ixj20fg0br74m.jpg)

其实在JS的世界里与`Vue`相似的库/框架满天飞，就像著名的`Angular`和`React`，但是在我都接触一段时间后，发现`Vue`才是我的真爱。好了，Let‘s Vue!

## 二、WHAT —— Vue简介

> `VueJs` 是一个**灵活**、**容易上手**的微型 **Javascript库**，开发者可以利用VueJs **非常简单**、**快速地** 开发出交互式的Web应用。VueJs和Angular, React, Ember, Polymer, and Riot等其他一些框架/库同中存异。但是相比下我认为VueJs **更简洁**、**更灵活**、**性能更高**。如果你感兴趣可以去[VueJs的官网](http://vuejs.org/)了解更多。
>
> ​							—— [千里之堤：VueJs基础](https://blog.j3n5en.com/2016/04/27/VueJs-The-Basics/)

这里再谈谈`Vue`的两大核心概念:

####  响应的数据绑定（双向数据绑定、Two-way-binding）

---

在jQuery的时代，我们常常使用 jQuery 手工操作 DOM。 我认为这经常重复还有就是很容易出错。而在`Vue`中__数据驱动视图__ 。也就是说，我们在HTML中通过特殊的指令将DOM与数据进行绑定。一旦创建了绑定，DOM 将与数据保持同步。每当修改了数据，DOM 便相应地更新。这样我们就不用再去操作DOM了。

demo

```html
<!-- 这是我们的 View -->
<div id="example-1">
  Hello {{ name }}!
</div>
```

```javascript
// 这是我们的 Model
var exampleData = {
  name: 'Vue.js'
}

// 创建一个 Vue 实例或 "ViewModel"
// 它连接 View 与 Model
var exampleVM = new Vue({
  el: '#example-1',
  data: exampleData
})
```



### 组件系统

---

组件系统是 Vue.js 另一个重要概念，因为它提供了一种抽象，让我们可以用独立可复用的小组件来构建大型应用。如果我们考虑到这点，几乎任意类型的应用的界面都可以抽象为一个组件树：

![](http://ww2.sinaimg.cn/large/6bf00bd8jw1f4dg0t4arfj21320f4wfc.jpg)



## Where ？

Here！and [官网教程](http://vuejs.org.cn/guide/)



> Happy Hacking !! && have a nice day!!

\#EOF\#

