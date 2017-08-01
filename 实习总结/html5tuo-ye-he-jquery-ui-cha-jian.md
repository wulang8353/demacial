## HTML5拖拽

### 基本知识点：

1. **DataTransfer**对象：拖拽对象用来传递的媒介，使用一般为`Event.dataTransfer`
2. **draggable**属性：就是标签元素要设置`draggable=true`，否则不会有效果，例如：
   ```
   <div draggable="true">拖拽</div>
   ```
3. **ondragstart**事件：当拖拽元素开始被拖拽的时候触发的事件，此事件作用在被拖曳元素上
4. **ondragenter**事件：当拖曳元素进入目标元素的时候触发的事件，此事件作用在目标元素上
5. **ondragover**事件：拖拽元素在目标元素上移动的时候触发的事件，此事件作用在目标元素上
6. **ondrop**事件：被拖拽的元素在目标元素上同时鼠标放开触发的事件，此事件作用在目标元素上
7. **ondragend**事件：当拖拽完成后触发的事件，此事件作用在被拖曳元素上
8. **Event.preventDefault\(\)**方法：阻止默认的些事件方法等执行。在`ondragover`中一定要执行`preventDefault()`，否则`ondrop`
   事件不会被触发。另外，如果是从其他应用软件或是文件中拖东西进来，尤其是图片的时候，默认的动作是显示这个图片或是相关信息，并不是真的执行`drop`。此时需要用在`document`的`ondragover`事件把它直接干掉。
9. **Event.effectAllowed**属性：就是拖拽的效果。



