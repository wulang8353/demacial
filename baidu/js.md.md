* [基础一](#basic)


  
> 有一个input输入框，以及4个操作按钮

> 点击"左侧入"，将input中输入的数字从左侧插入队列中；
点击"右侧入"，将input中输入的数字从右侧插入队列中；
点击"左侧出"，读取并删除队列左侧第一个元素，并弹窗显示元素中数值；
点击"右侧出"，读取并删除队列又侧第一个元素，并弹窗显示元素中数值；

>点击队列中任何一个元素，则该元素会被从队列中删除

两种思路：

* 定义input的值为数组，每次输入相当于推栈。使用For循环遍历数组，将每次输入的值赋值给创建的子节点上，使用setAttribute方法，在赋值的同时定义一个index属性使其的I，然后为子节点绑定删除事件。

* 每次插入子节点的时候，直接定义一个删除函数（这样做会创建很多函数，影响内存）

**内存和性能（事件委托）**：

{%  type= "#EC6149" em %}事件处理程序的数量将直接影响到页面整体的运行性能{% endem %}

1、 每个函数都是对象，都会占用内存

2、 内存中的对象越多，性能也就越差

3、 必须先指定所有事件处理程序而导致的DOM访问此，延迟交互就绪时间  

解决方案：**事件委托**

```
<input type="text" id="input" placeholder="请输入数值">
<span id="button">
    <button id='btnLeftIn'>左侧入</button>
    <button id='btnRightIn'>右侧入</button>
    <button id='btnLeftOut'>左侧出</button>
    <button id='btnRightOut'>右侧出</button>
</span>
<div id="content">

<script>

var btn = document.getElementById("button");
var input = document.getElementById("input");
var content = document.getElementById("content");
var list = [];   
    
// 为各个按钮绑定事件——事件委托
btn.onclick = function(event){
var event = event || window.event;
var target = event.target || event.srcElement;

// 当标签名是按钮时，通过判断id名，绑定对应的事件
if(target.tagName == "BUTTON"){ 
    switch(target.id){
    case "btnLeftIn":
        btnLeftIn();
        break;
    case "btnRightIn":
        btnRightIn();
        break;
    case "btnLeftOut":
        btnLeftOut();
        break;
    case "btnRightOut":
        btnRightOut();
        break;    
    }
}}

// 各个函数
function btnLeftIn() {
    if (!isNaN(input.value)) {
        list.unshift(input.value);
        insertValue(list);
    } else {
        alert("请输入数字哦")
    }
}

function btnRightIn() {
    if (!isNaN(input.value)) {
        list.push(input.value);
        insertValue(list);
    } else {
        alert("请输入数字哦")
    }
}

function btnLeftOut() {
    if (list.length) {
        list.shift();
        insertValue(list)
    } else {
        alert("没有值啦")
    }
}

function btnRightOut() {
    if (list.length) {
        list.pop();
        insertValue(list)
    } else {
        alert("没有值啦")
    }
}

// 将数组中的值插入到列表中
function insertValue(arr){
var length = arr.length;

for (var i=0; i<length;i++){
    var node = document.createElement("p");
    node.innerHTML = arr[i];
    content.appndChild(node);
    node.setAttribute("class",node);
    node.setAttribute("flag",i);
    }
}

// 当点击列表中的元素时，绑定相应事件
content.onclick = function(event){
    var flag = event.target.attribute("flag");
    this.removeChilde(event.target);
    list.splice(flag,1);   // 在数组中将点击的元素删掉，否则再次插入的时候，数组长度就因为没有少1而出错
    return list            // 必须返回数组
}
</srcipt>
```
这个方法在取值的时候是通过`input.value`，这里不方便对取到的值进行更一步的删选，所以用一个取值函数每次调用即可如下：
````
var btn = document.getElementById("button");
var input = document.getElementById("input");
var content = document.getElementById("content");
var list = []; 

// 每次点击产生事件的时候，都会将取到的值放入数组中，所以可对数组进行限定
function getValue(){
    var value = input.value;
    if(value == '' && isNaN(value)){
       alert('请输入正确的数值');  
    }esle {
        return value
    }
}
````
