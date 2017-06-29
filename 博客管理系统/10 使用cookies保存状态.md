## 用户注册、登陆

**Cookies为了记录当前状态，例如登陆状态等**

**原理：在http请求中创建Cookies对象，发送一个cookies给浏览器保存起来，以后再访问的时候(因为是根据对应url创建的对象，所以它会保存对应的url)，它会在验证url后将相关信息放入到请求头中传递给后端**


* 引入Cookies模块

```
/app.js

var Cookies = require('cookies'); //加载模块
...

// cookies设置,app.use中间件的存在使得用户访问后都会调用该方法,并且可以在模板引擎swig中直接调用改数据
app.use( function (req, res, next) {
    req.cookies = new Cookies(req, res);

    // 解析用户的登陆信息
    // 
    req.userInfo = {};
    if (req.cookies.get('userInfo')) {
        try {
            req.userInfo = JSON.parse(req.cookies.get('userInfo'));

            })
        } catch (e) {
             next();
        }
    } else {
         next();
    }

    // console.log( req.cookies.get('userInfo') );
    // 字符串类型 {"_id":"58fa0872ef1c5e4c085baf5e","username":"test"}
    // console.log( JSON.parse(req.cookies.get('userInfo')));
    // 对象类型   { _id: '58fa0872ef1c5e4c085baf5e', username: 'test' }

});
```

* 创建Cookies

```
// index.js

/*
 * 登录
 * */
router.post('/user/login', function(req, res) {


    // 设置cookies,第一个对象要保存的对象名，第二参数保存字符串存入到userInfo中
    // 数据会保存在req.cookies中
    req.cookies.set('userInfo', JSON.stringify({ 
        _id: userInfo._id,                       
        username: userInfo.username
    }));
    res.json(responseData);
    return;
  

});
```

当点击登陆时，会生成cookies保存在req中，可以在请求头中看到


![](/博客管理系统/img/cookies.jpg)