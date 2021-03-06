* [共享onload事件](#onload事件)

## 共享onload事件

假设文档中存在多个`a`标签，其中有某个`a`标签的类名是`popup`，为这个标签绑定事件，事件处理函数在`text.js`中，当浏览器加载文档时，能正常为该标签绑定事件：
```
// test.js
var links = document.getElementsByTagName("a");
for (var i = 0; i < links.length; i++) {
  if (links[i].getttribute('class') == 'popup') {
    links[i].onclick = function () {
      popup(this.getAttribute("href"));
      return false;  // 阻挡a标签的默认行为
    }
  }
}
```

在HTML页面中，我们有两种处理JavaScript代码的方式：

1 在文档的`<head>`部分用`<script>`标签调用

```
<head>
<meta charset="UTF-8">
<script type="text/javascript" src="test.js"></script>
</head>
...
```
**问题1**：浏览器顺序执行，当加载`text.js`文件时，浏览器会立即执行给Links变量赋值的语句。也就是说，`test.js`文件会在HTML文档之前先加载到浏览器中，这时，由于DOM结构还未被解析也就不存在`a`标签，赋值语句就会报错。

2 用`<script>`标签调用,然后位于文档底部`</body>`之前

**问题2**：由于浏览器可能一次加载多个脚本文件，所以脚本加载时，文档可能不完整，所以模型也不完整。没有完整的DOM，`getElementsByTagName()`方法也就不能正常工作

**解决方法**： **onload()**

原理：文档将被加载到一个浏览器窗口中，document对象又是window对象的一个属性。所以当window对象触发onload事件时，document对象已经存在了，也就是说DOM结构解析已完成。
```
// 这里我将代码打包放在links函数里,这样当浏览器加载完DOM后，就可以通过调用links方法直接为标签绑定事件了
window.onload = links; 
function links() {
  var links = document.getElementsByTagName("a");
  for (var i = 0; i < links.length; i++) {
      if (links[i].getttribute('class') == 'popup') {
          links[i].onclick = function() {
              popup(this.getAttribute("href"));
              return false; 
          }
      }
  }
}
```
但是，如果有多个函数，比如有links1(),links2()，想让他们都在页面加载时得到执行呢？

* 匿名函数包容这两个函数，然后将匿名函数绑定到onload事件上——针对要绑定的函数不是很多的场合

****

```
window.onload = function(){
  links1();
  links2();
}
```
* 弹性方法：封装

```
function addLoadEvent(func) {
    var oldonload = window.onload;
    if (typeof window.onload != 'function') {
        window.onload = func; // 处理函数上还没有绑定任何函数，那就添加新函数上去
    } else {
        window.onload = function() { // 若是已绑定了其他函数，那么通过匿名函数的方式追加到现有指令的末尾
            oldonload();
            func();
        }
    }
}
// 在js中直接调用即可
addLoadEvent(links1);
addLoadEvent(links2);
```