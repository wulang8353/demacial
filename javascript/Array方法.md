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

