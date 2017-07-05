# NodeJS项目线上部署

```
本地项目修改后 -> 上传到git仓库 -> 通过PM2来通知服务器进行更新

PM2：保证服务运行。当代码更新后，同步代码更新以及重启服务

```

## 向发布上线 Nodejs 项目

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


* 本地控制远端代码更新以及服务器重启

```
```
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目23.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目24.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目25.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目26.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目27.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目28.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目29.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目30.jpg)




