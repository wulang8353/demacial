### Static NodeList  Vs Live NodeList

#### 需求简介：

对于一个药品询价网站，不可避免地会经常遇到用户对多个商品列表的增删改查。这里在admin权限下对商品列表进行编辑，常规的方法就是拿到商品列表的id、勾选某个商品后的id，为删除图标绑定一个click事件，然后在事件中调用ajax方法，将拿到的id按照与后台统一商定的规范作为参数进行传入。

#### 问题描述：

由于框架选型使用了jQuery，所以很自然地使用$()来获取目标元素，然后使用事件委托绑定事件处理程序。
但是！我习惯性地遇到功能不太复杂的逻辑时，会试着用原生的js去复原。
于是问题就来了： **ul.getElementByTagName("li")在遍历时，length属性每次都会重新计算，导致删除非点击的元素**
先放示例代码：
````
<ul id="goods">
    <li>
        <div class="id">1</div>
        <div class="name">康泰</div>
        <div class="del">删除</div>
    </li>
    <li>
        <div class="id">1</div>
        <div class="name">泰诺</div>        
        <div class="del">删除</div
    </li>
    <li>
        <div class="id">1</div>
        <div class="name">阿莫西林</div>   
        <div class="del">删除</div>
    </li>
</ul>

var del = document.getElementsByClassName("del");
var ul = document.getElementById("goods");
var li = ul.getElementByTagName("li")

 for(var i = 0; i<del.length; i++) {
    ((i) => {
      del[i].addEventListener('click', () => {
        li[i].parentNode.removeChild(li[i])
        console.log("i=" + i)
        console.log("li.length= " + li.length)
      })
    })(i)
  }
````
![](/实习总结/imgs/static-1.jpg)

#### 问题分析
这里之所以会报错，是因为每一次调用 li都会重新对文档进行查询，数组长度会变化。
所以每次删除后，数组长度会变化，而外层for循环的i是根据最初数组的长度进行递增的。也就存在，当删除第二项的时候，i=1,而次时的数组只有两项[泰诺、阿莫西林]，所以删除的是li[1] = 阿莫西林。

![](/实习总结/imgs/static-2.jpg)

![](/实习总结/imgs/static-3.jpg)

#### 解决办法
采用querySelector方法，返回静态Node List
````
var del = document.getElementsByClassName("del");
var ul = document.getElementById("goods");
var li = ul.querySelectorAll("li");

 for(var i = 0; i<del.length; i++) {
    ((i) => {
      del[i].addEventListener('click', () => {
        li[i].parentNode.removeChild(li[i])
        console.log("i=" + i)
        console.log("li.length= " + li.length)
      })
    })(i)
  }
````

#### 原理分析
**getElementBy**等方法返回的是一个HTMLCollection对象，也是一个动态的Live Node List对象，每一次的调用都会重新对文档进行查询,基于DOM结构动态执行查询后的结果，因此DOM结构的变化会自动反映在Node List中，自动更新Length
**querySelectorAll**方法返回是Static Node Lis对象，是一个 li 集合的快照，指选出的所有元素的数组，不会随着文档操作而改变.不会更新自身Length

````
<ul>
    <li>111</li>
    <li>222</li> 
    <li>333</li> 
</ul>

var ul=document.querySelector('ul');
var list=ul.querySelectorAll('li'); 
for(var i=0;i<list.length;i++){ 
   ul.appendChild(document.createElement('li'));
}//这个时候就创建了3个新的li，添加在ul列表上。
 
console.log(list.length) //输出的结果仍然是3，不是此时li的数量6

var ul=document.getElementsByTagName('ul')[0];
var list=ul.getElementsByTagName('li');
for(var i=0;i<5;i++){ 
   ul.appendChild(document.createElement('li'));
} 

console.log(list.length)//此时输出的结果就是3+5=8
````
**getElementBy**与**querySelectorAll**的不同之处
1. W3C 标准
querySelectorAll 属于 W3C 中的 Selectors API 规范。而 getElementsBy 系列则属于 W3C 的 DOM 规范 

2、浏览器兼容
querySelectorAll 已被 IE 8+、FF 3.5+、Safari 3.1+、Chrome 和 Opera 10+ 良好支持 。getElementsBy 系列，以最迟添加到规范中的 getElementsByClassName 为例，IE 9+、FF 3 +、Safari 3.1+、Chrome 和 Opera 9+ 都已经支持该方法了

3、接收参数
querySelectorAll 方法接收的参数是一个 CSS 选择符。而 getElementsBy 系列接收的参数只能是单一的className、tagName 和 name

4、返回值
querySelectorAll 返回的是一个 Static Node List，而 getElementsBy 系列的返回的是一个 Live Node List

5、性能
querySelector可以使用css选择符来查找节点，相比getElemnetById+getElementByTagName这样复杂的操作要简单，但是querySelector查找范围会大很多，所以在性能上querySelector是被完爆的

#### 总结
