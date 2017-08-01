* [ES6](#ES6)

  * [Array.from](#Array.from)
  * [Array.of](#Array.of)

* [ES5](#ES5)

  * [forEach](#forEach)
  * [map](#map)
  * [filter](#filter)
  * [some](#some)
  * [every](#every)
  * [indexOf](#indexOf)
  * [lastIndexOf](#lastIndexOf)
  * [reduce](#reduce)

# ES6

## Array.from

对于IE6~IE8等浏览器，可以在Array原型上进行扩展

```
// 以forEach()为例
if(typeof Array.prototype.forEach  != "function"){
 Array.prototype.forEach = function(){...} 
}
```

# ES5

## forEach

* forEach为数组中的每个元素执行一次回调函数，**不会返回新的数组**

```
// js-forEach
[].forEach(function(value,index,array){
 ...
})

// jQuery—$.each()
$.each([],function(index,value,array){
 ...
})
```

第1个和第2个参数正好是相反的，类似有`map()`和`$.map()`

数组求和：

```
var sum = 0;

[1,2,3,4,5].forEach(function(v,i,array){
 console.log(v);
 sum +=v
})
alert(sum) //10
```

`forEach`函数接受一个必须的回调函数的同时，还可以接受一个可选的**上下文参数**\(改变回调函数中this的指向\)

```
var database = {
  users: ["张含韵", "江一燕", "李小璐"],
  sendEmail: function (user) {
    if (this.isValidUser(user)) {
      console.log("你好，" + user);
    } else {
      console.log("抱歉，"+ user +"，你不是本家人");    
    }
  },
  isValidUser: function (user) {
    return /^张/.test(user);
  }
};

// 给每个人法邮件
database.users.forEach(  // database.users中人遍历
  database.sendEmail,    // 回调函数，发送邮件
  database               // 上下文参数，使用database代替上面的this，否则就是全局对象代替
);

// 你好，张含韵
// 抱歉，江一燕，你不是本家人
// 抱歉，李小璐，你不是本家
```

对IE6-IE8进行仿真扩展

```
// this=实例，因为是数组调用，实例就是该数组
if(typeof Array.prototype.forEach !="function"){
  Array.prototype.forEach = function(fn,context){
    for(var i = 0,length = this.length;i++){
      if(typeof fun ==="function" && object.prototype.hasOwnProperty.call(this,i)){
        fn.call(context,this[i],i,this);
      }
    }
  }
}
```

## map

* 新数组
* 返回一个由原数组中的每个元素调用一个指定方法后的**返回值组成的新数组**

```
基本用法与forEach()类似
[].map(function(value, index, array) {
    // ...
});

// 例一
var data = [1, 2, 3, 4];
var arrayOfSquares = data.map(function (item) {
  return item * item;
});

alert(arrayOfSquares); // 1, 4, 9, 16

// 例二
var users = [
  {name: "张含韵", "email": "1@email.com"},
  {name: "江一燕",   "email": "2@email.com"},
  {name: "李小璐",  "email": "3@email.com"}
];

var emails = users.map(function (user) { return user.email; });

console.log(emails.join(","));// 1@email.com, 2@email.com, 3@email.com
```

对IE6-IE8进行仿真扩展

```
// this=实例
if(typeof Array.prototype.map !="function"){
  Array.prototype.map = function (fn,context){
    var arr = [];
    if(typeof fn === "function"){
      fo(rvar k = 0,length=this.length;k<length;k++){
        arr.push(fn.call(context,this[k],k,this));
      }
    }
    return arr;
  }
}
```

## filter

* 新数组
* 指数组filter后，**返回过滤后的新数组**,并且当回调函数返回的布尔值为true时，才会添加到新的数组中

```
var data = [0, 1, 2, 3];
var arrayFilter = data.filter(function(item) {
    return item;
});
console.log(arrayFilter); // [1, 2, 3]

// 弱等于== true/false
```

对IE6-IE8进行仿真扩展

```
if (typeof Array.prototype.filter != "function") {
  Array.prototype.filter = function (fn, context) {
    var arr = [];
    if (typeof fn === "function") {
       for (var k = 0, length = this.length; k < length; k++) {
          fn.call(context, this[k], k, this) && arr.push(this[k]);
          // context.fn执行后返回真 && 将通过的值传到数组中
       }
    }
    return arr;
  };
}
```

## some

* some意指“某些”，指是否“某些项”合乎条件,至少一个满足就返回true不再继续执行

```
var scores = [5,6,7,8];
var current = 7

function high (score){
  return score > current;
}

if(scores.some(high)){
  alter("满足要求")
}
```

对IE6-IE8进行仿真扩展

```
// js是弱类型语言，所以!会自动转换类型为boolean，两个叹号就等于Boolean('obj')
// 如果是一个object类型的变量，那只有未定义即等于undefined时才会为false，
// 这个实际是用来判断obj是否存在

if (typeof Array.prototype.some != "function") {
  Array.prototype.some = function (fn, context) {
    var passed = false;
    if (typeof fn === "function") {
         for (var k = 0, length = this.length; k < length; k++) {
          if (passed === true) break;
          passed = !!fn.call(context, this[k], k, this);
          // ！！ = boolean转型函数，使得返回值为布尔类型
      }
    }
    return passed;
  };
}
```

## every

* every需要每一项都满足才会返回true

```
var scores = [5,6,7,8];
var current = 7

function high (score){
  return score > current;
}

if(scores.some(high)){
  alter("满足要求")
}else{
  alert("不满足")
}
```

对IE6-IE8进行仿真扩展

```
if (typeof Array.prototype.some != "function") {
  Array.prototype.some = function (fn, context) {
    var passed = true;
    if (typeof fn === "function") {
         for (var k = 0, length = this.length; k < length; k++) {
          if (passed === false) break;
          passed = !!fn.call(context, this[k], k, this);
      }
    }
    return passed;
  };
}
```

## indexOf

对IE6-IE8进行仿真扩展

```
if (typeof Array.prototype.indexOf != "function") {
  Array.prototype.indexOf = function (searchElement, fromIndex) {
    var index = -1;
    fromIndex = fromIndex * 1 || 0;

    for (var k = 0, length = this.length; k < length; k++) {
      if (k >= fromIndex && this[k] === searchElement) {
          index = k;
          break;
      }
    }
    return index;
  };
}
```

## lastIndexOf

对IE6-IE8进行仿真扩展

```
if (typeof Array.prototype.lastIndexOf != "function") {
  Array.prototype.lastIndexOf = function (searchElement, fromIndex) {
    var index = -1, length = this.length;
    fromIndex = fromIndex * 1 || length - 1;

    for (var k = length - 1; k > -1; k-=1) {
        if (k <= fromIndex && this[k] === searchElement) {
            index = k;
            break;
        }
    }
    return index;
  };
}
```

## reduce

* 迭代器

```
array.reduce(callback[, initialValue])
```

callback函数接受4个参数：之前值、当前值、索引值以及数组本身。initialValue参数可选，表示初始值。若指定，则当作最初使用的previous值；如果缺省，则使用数组的第一个元素作为previous初始值，同时current往后排一位，相比有initialValue值少一次迭代。

```
var sum = [1, 2, 3, 4].reduce(function (previous, current, index, array) {
  return previous + current;
});

console.log(sum); // 10

// 因为initialValue不存在，因此一开始的previous值等于数组的第一个元素。
// 从而current值在第一次调用的时候就是2.
// 最后两个参数为索引值index以及数组本身array.
```

循环过程：

```
// 初始设置
previous = initialValue = 1, current = 2

// 第一次迭代
previous = (1 + 2) =  3, current = 3

// 第二次迭代
previous = (3 + 3) =  6, current = 4

// 第三次迭代
previous = (6 + 4) =  10, current = undefined (退出)
```

有了`reduce`，我们可以轻松实现二维数组的扁平化：

// 二维数组扁平化

```
var matrix = [
  [1, 2],
  [3, 4],
  [5, 6]
];
var flatten = matrix.reduce(function (previous, current) {
  return previous.concat(current);
});

console.log(flatten); // [1, 2, 3, 4, 5, 6]
```

// 多维数组扁平化

```
function init (arr) {
    var newArr = [];
    if (arr instanceof Array) {
        arr.forEach(function (value) {
            newArr = newArr.concat(init(value));
        });
    } else {
        newArr.push(arr);
    }
    return newArr;
}

init([1, [2], [3, [[4]]]]); // 1,2,3,4
init([1, [], [3, [[4]]]]);  // 1,3,4
```



