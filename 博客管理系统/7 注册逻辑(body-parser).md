## 用户注册、登陆

** [body-parser](https://github.com/expressjs/body-parser) 中间件，专门处理http请求的数据**

整理下思路：
1 首先由main.js中的express.Router()定位到页面路径并渲染，然后在页面上点击注册 -> 
2 根据head导入的index.js找到ajax请求的url ->
3 由api中express.Router()定位到请求的url，并进行逻辑验证

**express.Router()处理有关url方面的逻辑(页面、http)，通过Body-parser处理http中请求的数据**

* 在app.js中调用body-parse

```
var bodyParse = require('body-parser'); // 用来处理POST提交过来的数据

// bodyParse设置
app.use( bodyParse.urlencoded({extended: true})); // 在req对象上绑定一个body属性，得到发送过来的数据对象
```
* 在/router/api.js 中处理请求数据的逻辑

```
//统一返回格式
var responseData;

router.use( function(req, res, next) {
    responseData = {
        code: 0,  // 错误码
        message: ''
    };

    next();  // 继续执行后续的函数 否则就会挂起。
} );
/*
 * 用户注册
 *   注册逻辑
 *
 *   1.用户名不能为空
 *   2.密码不能为空
 *   3.两次输入密码必须一致
 *
 *   > 用户是否已经被注册了
 *       数据库查询
 *
 * */
var express = require('express');
var router = express.Router();

router.post('/user/register', function(req, res, next) {

     // console.log( req.body );  { username: 'node', password:'123', repassword:'123' }
     // bodyParser 专门处理post提交的请求：在req自动添加一个body属性
    var username = req.body.username;
    var password = req.body.password;
    var repassword = req.body.repassword;

    //用户是否为空
    if ( username == '' ) {
        responseData.code = 1;
        responseData.message = '用户名不能为空';
        res.json(responseData); // 将这个对象转换成JSON格式返回到前端
        return;                 // 终止代码继续执行
    };
    //密码不能为空
    if (password == '') {
        responseData.code = 2;
        responseData.message = '密码不能为空';
        res.json(responseData);
        return;
    };
    //两次输入的密码必须一致
    if (password != repassword) {
        responseData.code = 3;
        responseData.message = '两次输入的密码不一致';
        res.json(responseData); // {"code":3,"message":"两次输入的密码不一致"}
        return;
    };

    responseData.message = '注册成功';
    res.json(responseData);
    return;
});
```
输入用户名和密码后，点击注册，就可以在运行的node窗口中看到提交的注册信息，并在控制台输出对应信息
![](/博客管理系统/img/body-parser2.jpg)

![](/博客管理系统/img/body-parser1.jpg)

![](/博客管理系统/img/body-parser5.jpg)


当未输入用户名的时候，显示用户名不能为空
![](/博客管理系统/img/body-parser11.jpg)

![](/博客管理系统/img/body-parser4.jpg)














