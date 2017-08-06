## 一、项目简介

该项目是在一个需要付费的用于药价查询的询价网站，进行重构。由于之前性能相当差，查询搜索的响应时间大于12秒，保存下载时系统甚至会崩溃，因此要先重构了两个大模块，叫做Pricentric Utra Light（简称UL），现在要做Re-engineering是基于UL的基础上，对数据库以及前端代码上进行重构的，最主要的是解决性能问题。

### 1、项目前端搭建

* 使用MVC框架angular\(指定\)，引入angular-ui-router、angular-file-save等其他ng插件

* 使用Bootsrap和jQuery设计网页布局和样式处理

* 前后端的数据请求交互通过Ajax完成

* 引入了Moment.js格式化前端页面显示时间

### 2、项目后端搭建

## 二、框架选取

前端采用MVC框架angular，无疑会在性能上得到显著提高，其强大的双向数据绑定、依赖注入、指令复用等功能较于传统的系统而言，能减轻代码冗余程度，其次DOM操作的减少也会提高系统性能，增强用户体验。

目前现在前端框架由**Vue、React、Angular2**三足鼎立，Angular2还没涉及，React总体感觉通篇都是JS但是Native移动端确实吸粉无数，下面主要谈下Vue

> Angular的适用领域相对窄一些，React可以拓展到服务端，移动端Native部分，而Vue因为比较轻量，还能用于业务场景非常轻的页面中。

* \[**Vue**\]\([https://cn.vuejs.org/v2/guide/](https://cn.vuejs.org/v2/guide/)** **\)

官网介绍：Vue是一套构建用户界面的**渐进式框架 **，他与其他重量级框架不同的是，Vue 采用自底向上增量开发的设计。Vue 的核心库只关注视图层，它不仅易于上手，还便于与第三方库或既有项目整合。但我在学过React，用过angular1后，觉得它的学习成本是最低，这也就代表着上手的程度快。

1、 Vue模板

Vue基于HTML、CSS进行扩展和封装，形成的自己的Vue模板，这一点和React不同，React的所有组件的渲染功能都依靠 JSX\(JSX 是使用 XML 语法编写 JavaScript 的一种语法糖\) 这对于习惯HTML的我来说，前期是比较难受的。而且这对于与既有项目或者第三方库的整合是具有一定的难度

2、CSS组件作用域

简单来说就是一个.vue页面由html、css、js组成，也就是一个组件，而一个组件中有它自己的样式，修改和调试的时候一目了然。这也就代表着组件和组件之间的样式不会互相干扰。**天知道！原来用angular做项目的时候，初期为了省事就把样式写在一张表上，到后面也越写越多，等到后面意识到不对的时候，已经救不会来了~~**

3、组件复用

举个例子，比如曾经自己做的Demo中有一个五星评分的结构，而这个在几个页面中都有用到，按照传统的方式，我应该会把HTML代码复制粘贴几次。但是正如我说的，一个页面就是一个组件，换句话说，**哪个地方需要这个功能，就可以直接引入这个组件**，Vue会管理好引入的组件，让其正确且完整的显示在页面当中

4、强大的路由

页面之间的跳转向来是一件头疼的事情，有时候想拿到路由中的参数进行某些操作的时候，层级不深的就还好，但是对于层级较深，hash值又臭又长的情况，Vue的vue-router解决了以上问题，并且它通过制定一个router对象，让页面的跳转路径清晰的对应起来，类似还有动态路由、嵌套路由等功能，大大方便对url的操作

5、HTTP请求

这里推荐axios ：Promise based HTTP client for the browser and node.js.  采用ES6的Promise异步操作进行Http请求和调用本地Mock数据，还非常贴心的tranforms JSON data

6、vue-cli脚手架

统一项目的目录结构，这就意味着从原来各式各样的文件名中解放出来啦~通过webpack打包工具，增添了热加载，jslint，sass、less、甚至bootstrap等美好的功能

7、基于HTML、CSS、jQuery的设计框架bootstrap，基于vue的设计框架ui-element，当然也有基于react的设计框架antd

[https://cn.vuejs.org/v2/guide/comparison.html](https://cn.vuejs.org/v2/guide/comparison.html)

[https://github.com/vuejs/vue-cli](https://github.com/vuejs/vue-cli)

## 三、性能优化

性能优化的一般原则有两点：

1 减少Http请求，使用内存、缓存
2 减少CPU计算

针对这两点原则，可以从两方面入手

* 1、加载页面和加载静资源（资源优化）

（1）静态资源的压缩合并：CSS sprites, JS、CSS代码压缩
（2）静态资源缓存
（3）使用CDN让资源加载更快，图片服务器
（4）使用SSR后端渲染，数据直接输出到HTML中

* 2、页面渲染（渲染优化）

（1）CSS放前，JS放后
（2）懒加载（图片、列表懒加载、下拉加载更多）
（3）减少DOM查询，对DOM查询做缓存
（4）减少DOM操作，多个操作尽量合并在一起执行
（5）事件节流
（6）尽早执行操作（DOMContentLoaded,$(document).ready(...)）
（7）避免使用空的src和href



