## 文件系统模块

> 该模块是核心模块，需要使用require导入后使用
> * 异步形式

>    回调的参数,如果操作成功完成，则第一个参数会是 null 或 undefined
     **操作不会阻塞后续代码执行**
>* 同步形式

>    任何异常都会被立即抛出。 可以使用 try/catch 来处理异常，或让它们往上冒泡
     **操作阻塞后续代码执行**
     
 * [文件系统模块](#文件系统模块)
     * [fs.open](#fs.open)
     * [fs.read](#fs.read)
     * [fs.write](#fs.write)
     * [fs.writeFile](#fs.writeFile)
     * [fs.readFile](#fs.readFile)
     * [fs.watch](#fs.watch)
     * [dir文件夹](#dir文件夹)



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
> readSync() 同步读取
> 将读取到的数据存入缓冲对象buffer中，原文件不变

```
var fs = require('fs');

// 1.text 默认内容：abcd
fs.open('1.txt', 'r', function(err, fd) {

    if (err) {
        console.log('文件打开失败');
    } else {     
        /*
        * fs.read(fd, buffer, offset, length, position, callback)
        *   读取不影响原来文件
        *   fd : 通过open方法成功打开一个文件返回的编号
        *   buffer : buffer对象  数据写入的缓冲区
        *   offset : 缓冲区buffer中的写入偏移量
        *   length ： 要从文件中读取的字节数
        *   position ：读取的文件中的起始位置
        *   callback : 回调
        *       err
        *       len: 被读取文件的长度 fd => 1.text1
        *       buffer: 缓冲区对象
        * *

        var bf1 = new Buffer('123456789');

        console.log(bf1); 
        // <Buffer 31 32 33 34 35 36 37 38 39>

        fs.read( fd, bf1, 0, bf1.length, null, function( err, len, newBf ) {

            console.log( bf1 );
            // <Buffer 61 62 63 64 35 36 37 38 39>
            
            console.log( len );
            // 4
            
            console.log( newBf );
            // <Buffer 61 62 63 64 35 36 37 38 39> 
            //  text内容：abcd

        } );
    }
});
```

### fs.write

> write() 异步写入文件 
> writeSync() 同步写入
> 如果文件存在，该方法写入的内容会覆盖旧的文件内容。
> **针对打开的文件进行操作(需要传入fd)**

```
var fs = require('fs');

// 1.text 默认内容：abcd
fs.open('1.txt', 'r+', function(err, fd) {

    /*
    * 当我们要对打开的文件进行写操作的时候，打开文件的模式应该是  读写方式r+
    * 将定义在buffer中的数据写入到文件中
    *
    * fs.write(fd, buffer, offset, length[, position], callback)
    *   fd : 打开的文件
    *   buffer : 存入数据的缓冲对象
    *   offset : 写入buffer数据的起始位置
    *   length : 写入buffer数据的长度
    *   position : 操作文件fd中的起始位置  超出填充空格
    *   callback : 回调
    *        err
    *        len:buffer的长度
    *        buffer对象
    * */

    if (err) {
        console.log('打开文件失败')
    } else {

        var bf = new Buffer('123');

        // 1 对字符串截取添加
        fs.write( fd, bf, 0, 3, 5, function() {
            console.log(arguments);
            // { '0': null, '1': 2, '2': <Buffer 31 32 33> }
        } );
        //   text内容：abcd 123
        
        // 2 直接将数据写入到文件中
        // fs.write( fd, '1234', 5, 'utf-8' );
        // text 默认内容：abcd
        // text内容：abcd 1234
    }
});
```

### fs.writeFile

> 对文件进行写入(可以直接创建文件，无需fd)

```
var fs = require('fs');

/*
* 向一个指定的文件中写入数据，如果该文件不存在，则新建，如果存在则新的内容会覆盖原有的文件内容
* */

var filename = '2.txt';
fs.writeFile(filename, 'hello', function() {

    fs.writeFile(filename, data,  callback)
    * fd : 文件名
    * data: 写入的数据
    * callback : 回调
    
    console.log(arguments);
    // { '0': null }  
    // 并创建了文件2.txt，内容为 hello  
})

// 在原有文件的基础上追加新内容(不存在也会重新创界)
fs.appendFile(filename, '-leo', function() {
     console.log(arguments);
     // { '0': null }
     // 内容为 hello-leo  
 })

// 检查指定路径的文件是否存在
fs.exists( filename, function(isExists) {
    //console.log(isExists);  返回Boolean值

    if (!isExists) {
        fs.writeFile(filename, 'miaov', function(err) {
            if (err) {
                console.log('出错了');
            } else {
                console.log('创建新文件成功');
            }
        })
    } else {
        fs.appendFile(filename, '-leo', function(err) {
            if (err) {
                console.log('新的内容追加失败');
            } else {
                console.log('新内容追加成功');
            }
        })
    }

} );

// 同步处理，且没有函数没有返回值
if ( !fs.existsSync(filename) ) {
    fs.writeFileSync(filename, 'miaov');
    console.log('新文件创建成功');
} else {
    fs.appendFileSync(filename, '-leo')
    console.log('新内容追加成功')
}

```

### file.readFile

> 直接读取文件的全部内容

```
var fs = require('fs');

// 1.text 原内容 hello
fs.readFile('1.txt', function(err, data) {
    fs.readFile(filename[, options],callback)
    * filename: 文件名
    * callback : 回调
        err
        data 字符编码未指定，则返回原始的 buffer

    // console.log(arguments);
    // { '0': null, '1': <Buffer 68 65 6c 6c 6f> }
    
    if (err) {
        console.log('文件读取失败');
    } else {
        console.log( data.toString() ); // hello
    }
});

// 删除文件
fs.unlink('2.txt', function(err) {
    if (err) {
        console.log('删除失败');
    } else {
        console.log('删除成功');
    }
})

// 重命名
fs.rename('2.txt', '2.new.txt', function() {
    console.log(arguments);
})

// 状态信息查看
fs.stat('2.new.txt', function() {
    fs.stat(filename[, options],callback)
    * filename: 文件名
    * callback : 回调
        err 
        info 文件的信息，包括mode
    console.log(arguments);
})
```

### fs.watch

> 监听文件\文件夹状态变化
> 当filename是文件类型，监控文件的变化
> 当filename是文件夹类型，需要调用fs.readdir()遍历出里面所有的文件

```
var fs = require('fs');

var filename = '2.new.txt';

fs.watch(filename, function(ev, fn) {
    fs.watch(filename[, options][, listener])
    *options <String> | <Object>
      persistent <Boolean> 指明如果文件正在被监视，进程是否应该继续运行。默认 = true
      recursive <Boolean> 指明是否全部子目录应该被监视.默认 = false
      encoding <String> 指定用于传给监听器的文件名的字符编码。默认 = 'utf8'
    * listener <Function>
        eventType  rename | change
        filename   触发事件的文件的名称
        
    if (fn) {
        console.log(fn + ' 发生了改变');
    } else {
        console.log('....');
    }
});
```
### dir文件夹

```
var fs = require('fs');

// 创建一个当前目录的文件夹
fs.mkdir('./1', function() {
    
    fs.mkdir(path, [mode], callback)
    
    console.log(arguments);
});

// 遍历文件类型
fs.readdir('../FileSystem', function(err, fileList) {
    fileList.forEach(function(f) {

        fs.stat(f, function(err, info) {
            switch (info.mode) {
                case 16822:
                    console.log( '[文件夹] ' + f );
                    break;

                case 33206:
                    console.log( '[文件] ' + f );
                    break;

                default :
                    console.log( '[其他类型] ' + f );
                    break;
            }
        });
    });
})
```



