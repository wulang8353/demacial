## 文件系统模块

> 该模块是核心模块，需要使用require导入后使用
> * 异步形式

>    回调的参数,如果操作成功完成，则第一个参数会是 null 或 undefined
>* 同步形式

>    任何异常都会被立即抛出。 可以使用 try/catch 来处理异常，或让它们往上冒泡

```
var fs = require('fs');

/*
* fs.open(path, flags, [mode], callback)
*   path : 要打开的文件的路径
*   flags : 打开文件的方式 读/写
*   mode : 设置文件的模式 读/写/执行  4/2/1
*   callback : 回调
*       err : 文件打开失败的错误保存在err里面，如果成功err为null
*       fd : 被打开文件的标识，和定时器(涉及到文件操作，当前打开文件编号)
* */

fs.open('1.txt', 'r', function(err, fd) {

    // console.log(err);  错误信息 | null
    // console.log(fd);

    if (err) {
        console.log( '文件打开失败' );
    } else {
        console.log( '文件打开成功' );
        console.log( fd );
    }

});

fs.open('1.txt', 'r', function(err, fd) {
    console.log(fd);
});

```