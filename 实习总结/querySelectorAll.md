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

#### 原理分析
**getElementBy**等方法返回的是一个HTMLCollection对象，这是一个动态的Live Node List，每一次的调用都会重新对文档进行查询,更新自身Length
**querySelectorAll**方法返回是Static Node Lis对象，是一个 li 集合的快照，对文档的任何操作都不会对其产生影响,不会更新自身Length

实际上，HTMLCollection 和 NodeList 十分相似，都是一个动态的元素集合，每次访问都需要重新对文档进行查询。两者的本质上差别在于，HTMLCollection 是属于 Document Object Model HTML 规范，而 NodeList 属于 Document Object Model Core 规范


````
var ul = document.getElementsByTagName('ul')[0],
    lis1 = ul.childNodes,
    lis2 = ul.children;
console.log(lis1.toString(), lis1.length);    // "[object NodeList]" 11
console.log(lis2.toString(), lis2.length);    // "[object HTMLCollection]" 4
````
