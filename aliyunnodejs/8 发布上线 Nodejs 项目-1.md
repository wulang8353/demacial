# NodeJS项目线上部署

```
本地项目修改后 -> 上传到git仓库 -> 通过PM2来通知服务器进行更新

PM2：保证服务运行。当代码更新后，同步代码更新以及重启服务

```

## 向发布上线 Nodejs 项目-1

上传并发布静态项目app.js
```
const http = require('http')

const homePage = `
<!DOCTYPE html>
<html>
  <head>
    <meta charset='utf-8'>
    <title>Nodejs 部署上线示例（随时失效）</title>
    <style type="text/css">
      * {
        padding: 0;
        margin: 0;
      }

      body {
        padding: 30px 0;
        text-align: center;
        font-size: 16px;
        background-color: #333;
      }

      h1,
      h2 {
        color: #fff;
      }

      nav {
        margin-top: 20px;
      }

      a {
        color: #ccc;
        text-decoration: none;
      }

      a:hover {
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <h1>慕课网 Nodejs 高级课程案例（新增案例）</h1>
    <h2>项目部署上线示例</h2>
    <nav>
      <ul>
        <li>
          <a target="_blank" href="/a">Nodejs 电影网站</a>
        </li>
        <li>
          <a target="_blank" href="/b">狗狗说 App 后台</a>
        </li>
        <li>
          <a target="_blank" href="/c">微信小程序后台</a>
        </li>
        <li>
          <a target="_blank" href="/d">微信公众号后台</a>
        </li>
      </ul>
    </nav>
  </body>
</html>
`

http.createServer((req, res) => {
  res.statusCode = 200
  res.setHeader('Content-Type', 'text/html')
  res.end(homePage)
})
.listen(3000, () => {
  console.log('Server Running At 3000')
})

```

* 创建代码托管仓库

创建私有仓库，这里推荐[码云](http://git.oschina.net/)作为git仓库

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目1.jpg)

在本地的administartor路径下，找到本地公钥
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目2.jpg)

通过cat命令打开公钥后，将里面的内容复制到码云上
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目3.jpg)

* 本地上传到码云仓库

点击刚才创建的仓库，点击页面右方有一个克隆/下载，点击复制ssh
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目4.jpg)

来到本地项目的命令行中执行

```
git init  // 初始化项目

git add .  

git commit -m 'fisrt commit' 

```

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目5.jpg)

```
git remote add origin git@git.oschina.net:...(这里填写刚才在网页中复制过的ssh)
// 将本地项目关联到码云的仓库中
```
git push -u origin master 上传本地文件至远程仓库中，但是由于远程仓库中含有创建的LICENS文件，所以上传失败
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目6.jpg)

```
git fetch // 将远端仓库的内容下载到本地
```

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目11.jpg)

```
git merge origin/master --allow-unrelated-histories // 合并本地文件以及下载到本地远端仓库中的文件
```

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目8.jpg)

回车后会到以下的界面， 退出保存即可
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目7.jpg)

git push -u origin master 上传本地文件至远程仓库中
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目9.jpg)

来往码云的仓库中，即可发现已经上传成功
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目10.jpg)

* 远程仓库的代码上传至服务器

同样将服务器的公约保存到码云中
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目12.jpg)

先在服务器中安装git
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目13.jpg)

在根路径下新建temp文件，保存从git仓库下载项目，然后通过git clone <ssh> 下载项目
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目14.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目15.jpg)

* 同步代码更新

[PM2官方文档](http://pm2.keymetrics.io/docs/usage/deployment/)

本地项目中需要创建一个PM2的配置文件

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目22.jpg)

```
// ecosystem.json

{
  "apps": [
    {
      "name": "Website",
      "script": "app.js",
      "env": {
        "COMMON_VARIABLE": "true"
      },
      "env_production": {
        "NODE_ENV": "production"
      }
    }
  ],
  "deploy": {
    "production": {
      "user": "你的服务器登录用户名",
      "host": ["你的服务器 IP"],
      "port": "你的服务器登录端口", 
      "ref": "origin/master",
      "repo": "你的项目git仓库的ssh",
      "path": "/www/website/production",
      "ssh_options": "StrictHostKeyChecking=no",
      "env": {
        "NODE_ENV": "production"
      }
    }
  }
}
```

在服务器的根路径下创建www文件夹
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目16.jpg)

进入到www文件中，创建website文件夹
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目17.jpg)

通过cd .. 回退到上一级，也就是 www路径下
sudo chmod 777 website 提升website文件夹的权限，**使其在非root管理员用户下，可读可写可执行**
可以看到website文件夹颜色变化了
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目19.jpg)

由于服务器上已经按照deploy中的path配置好相关路径 /www/website

回到本地项目中，执行pm2，将本地文件上传到服务器中

```
pm2 deploy ecosystem.json production setup // 通过配置文件，指定项目安装到服务器的路径、端口等
```

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目20.jpg)

现在可以在服务器的website文件夹中看到上传成功的文件

    current : 当前的服务所运行的文件夹
    source: 源代码
    shared：日志等共享的文件
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目21.jpg)


* 本地通知服务器发布代码

```
本地修改代码 -> 上传到git仓库 -> 本地通过pm2 deploy ecosystem.json production 来通知服务器上对应的某个端口的项目，告诉它快去git仓库看看有没有更新
```

由于在服务器端PM2是非交互式的ssh连接方式，需要修改配置文件 .bashrc
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目23.jpg)

进入到.bashrc文件中，如需图所示添加注释，退出保存
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目24.jpg)

保存后，通过source .bashrc重新加载配置
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目25.jpg)

在本地命令行，通过pm2发布项目，通知服务器进行更新，这时候尾部没有setup
但是此时报错，是因为我们新增的ecosystem.json文件未上传到git仓库中，服务器未找到该文件
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目26.jpg)

一顿操作后，添加到git仓库
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目27.jpg)

添加 pm2 deploy ecosystem.json production来发布项目
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目28.jpg)

通过pm2 list 查看服务器正在运行的项目
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目29.jpg)

* 域名访问网站

    1 修改域名指向，可以通过域名访问网站，得到web服务
    2 修改nginx配置文件，把请求转发到服务器的3000端口(json文件中默认的是3000端口)

首先确保DNSPod中含有以 www开头的域名解析规则
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目32.jpg)

进入到服务器端的/etc/nginx/conf.d文件夹中，将原来的配置文件更换名称
命名规则为 www-域名-端口.conf
更换成功后通过vi 进入到文件中
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目30.jpg)

```
upstream webstie  与 proxy_pass http://website   对应！
server 中的端口为json文件中的3000d端口
server_name: 为解析的域名规则
退出保存
```
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目31.jpg)

重载nginx配置
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目36.jpg)

由于设置了iptables，未允许3000端口的访问，所以现在修改防火墙配置
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目37.jpg)

增加3000端口的访问规则
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目38.jpg)

重载防火墙配置
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目39.jpg)

通过域名即可访问到页面
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目40.jpg)

* 本地修改项目，服务器更新代码

```
当本地修改项目代码后只需要三步就可以通知服务器更新
```
1 修改代码

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目41.jpg)

2 提交代码到git仓库
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目42.jpg)

3 本地发布项目，服务器自动去git仓库更新代码
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目43.jpg)

这里刷新页面后就可以看到修改后的内容
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目43.jpg)