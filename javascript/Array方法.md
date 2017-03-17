* [ES5](#ES5)
 * [forEach](#forEach)
 * [map](#map)
 * [filter](#filter)
 * [some](#some)
 * [every](#every)
 * [indexOf](#indexOf)
 * [lastIndexOf](#lastIndexOf)
 * [reduce](#reduce)
 * [reduceRight](#reduceRight)

对于IE6~IE8等浏览器，可以在Array原型上进行扩展
```
// 以forEach()为例
if(typeof Array.prototype.forEach  != "function"){
 Array.prototype.forEach = function(){...} 
}
```
# ES5
## forEach()

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

`forEach`函数接受一个必须的回调函数的同时，还可以接受一个可选的**上下文参数**(改变回调函数中this的指向)

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
// this=实例，因为是数组调用
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


















