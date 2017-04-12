## 文件系统模块

> 该模块是核心模块，需要使用require导入后使用
> * 异步形式

>    回调的参数,如果操作成功完成，则第一个参数会是 null 或 undefined
     **操作不会阻塞后续代码执行**
>* 同步形式

>    任何异常都会被立即抛出。 可以使用 try/catch 来处理异常，或让它们往上冒泡
     **操作阻塞后续代码执行**

### fs.open

> open() 异步打开文件 
> 
> openSync() 同步打开

```
var fs = require('fs');

/*
* fs.open(path, flags, [mode], callback)  | 
*   path : 要打开的文件的路径 默认同一目录下某个文件
*   flags : 打开文件的方式 — 读/写 r:只读 r+ 可读写
*   mode : 设置文件的模式 读/写/执行  4/2/1
*   callback : 回调
*       err : 文件打开失败的错误保存在err里面，如果成功err为null
*       fd : 被打开文件的标识，和定时器(涉及到文件操作，每次打开的文件其编号不一样)
* */

fs.open('1.txt', 'r', function(err, fd) {

    if (err) {
        console.log( '文件打开失败' );
    } else {
        console.log( '文件打开成功' );
        console.log( fd ); 举例 3
    }
    
});

fs.open('1.txt', 'r', function(err, fd) {
    console.log(fd);  举例 4 重复添加
});

```

### fs.read

> read() 异步读取文件 
> 
> readSync() 同步读取


```
var fs = require('fs');

fs.open('1.txt', 'r', function(err, fd) {

    if (err) {
        console.log('文件打开失败');
    } else {
        /*
        * fs.read(fd, buffer, offset, length, position, callback)
        *   fd : 通过open方法成功打开一个文件返回的编号
        *   buffer : buffer对象
        *   offset : 新的内容添加到buffer中的起始位置
        *   length ： 添加到buffer中内容的长度
        *   position ：读取的文件中的起始位置
        *   callback : 回调
        *       err
        *       len:buffer的长度
        *       buffer对象
        * */

        var bf1 = new Buffer('123456789');

        console.log(bf1); 
        // 

        fs.read( fd, bf1, 0, 4, null, function( err, len, newBf ) {

            console.log( bf1 );
            //
            console.log( len );
            //
            console.log( newBf );
            //

        } );

    }

});
```