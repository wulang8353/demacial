## 前端项目自动化

> 运行Node脚本，完成项目目录的创建

```
var projectData = {

    'name' : 'hello',
    'fileData' : [
        {
            'name' : 'css',
            'type' : 'dir'
        },
        {
            'name' : 'js',
            'type' : 'dir'
        },
        {
            'name' : 'images',
            'type' : 'dir'
        },
        {
            'name' : 'index.html',
            'type' : 'file',
            'content' : '<html>\n\t<head>\n\t\t<title>title</title>\n\t</head>\n\t<body>\n\t\t<h1>Hello</h1>\n\t</body>\n</html>',
        }
    ]

};

var fs = require('fs');

if ( projectData.name ) {            // 确定文件名
    fs.mkdirSync(projectData.name);

    var fileData = projectData.fileData;

    if ( fileData && fileData.forEach ) {

        fileData.forEach(function(f) {

            // 生成路径 hello/name
            f.path = projectData.name + '/' + f.name; 

            f.content = f.content || '';

            switch (f.type) {

                case 'dir':
                    fs.mkdirSync(f.path);
                    break;

                case 'file':
                    fs.writeFileSync(f.path, f.content);
                    break;

                default :
                    break;
            }
        });
    }
}
```
* 生成的目录结构

![目录](/Nodejs/img/content.png)

* index.html内容

![页面](/Nodejs/img/index.png)



```
/**
 * Created by 毅 on 2015/9/20.
 */

var fs = require('fs');

var filedir = './content/source';

fs.watch(filedir, function(ev, file) {

// 只要有一个文件发生了变化,就需要对这个文件夹下的所有文件进行读取，然后合并

    fs.readdir(filedir, function(err, dataList) {

        var arr = [];
        dataList.forEach( (f) => {
           if (f) {
               var info = fs.statSync(filedir + '/' + f)// 同步stat，传入正确的路径，返回值就是所有参数

               if (info.mode == 33206) {
                   arr.push(filedir + '/' + f);
               }
           }

        });

        var content = '';
        arr.forEach( (f) => {
            var c = fs.readFileSync(f);

            content += c.toString() + '\n';

        });
        console.log(content);

        // 读取数组中的文件内容，并合并
        fs.writeFile('./content/js/index.js', content);

    });
});
```

