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
        list.unshift(<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>基础JavaScript练习（一）</title>
    <style type="text/css">
    .input {
        width: 150px;
        border: 1px solid #eaece9;
    }

    button {
        border: 1px solid #eaece9;
        background: #eee;
        border-radius: 4px;
        cursor: pointer;
    }

    .node {
        padding: 8px;
        background: #fb0200;
        color: #fff;
        float: left;
        margin: 10px 5px 0 0;
        cursor: pointer;
    }
    </style>
</head>

<body>
    <input type="text" id="input" placeholder="请输入数值">
    <span id="button">
        <button id='btnLeftIn'>左侧入</button>
        <button id='btnRightIn'>右侧入</button>
        <button id='btnLeftOut'>左侧出</button>
        <button id='btnRightOut'>右侧出</button>
    </span>
    <div id="content">
    </div>
    <script type="text/javascript">
    var btn = document.getElementById("button");
    var input = document.getElementById("input");
    var content = document.getElementById("content");
    var list = [];

    //  插值
    function insertValue(arr) {
        var length = arr.length;
        content.innerText = ' ';

        for (var i = 0; i < length; i++) {
            var node = document.createElement("p");
             node.innerText = arr[i];
            content.appendChild(node);
            node.setAttribute("class", "node");
            node.setAttribute("flag", i);
        }
    }

    // 点击列表中的元素绑定事件
    content.onclick = function(event) {
        var flag = event.target.getAttribute("flag");
        this.removeChild(event.target) // 移除点击的
        list.splice(flag, 1);
        return list
    }

    // 使用事件委托
    btn.onclick = function(event) {
        var event = event || window.event;
        var target = event.target || event.srcElement;

        if (target.tagName.toLowerCase() == 'button') {
        // 访问元素的标签名时，这里回出现全部大写的情况
            switch (target.id) {
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
        }
    }

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
    </script>
</body>

</html>
);
        insertValue(list);
    } else {
        alert("请输入数字哦")
    }
}

function btnRightIn() {
    if (!isNaN(input.value)) {
        list.push();
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



</srcipt>
```
