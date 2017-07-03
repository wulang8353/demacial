# NodeJS项目线上部署

## 搭建 Nodejs 生产环境

更新服务器
![](/aliyunnodejs/imgs/Node生产环境1.jpg)

安装服务器环境所依赖的包
![](/aliyunnodejs/imgs/Node生产环境2.jpg)

在github上的[nvm](https://github.com/creationix/nvm)，复制Wget的安装路径

![](/aliyunnodejs/imgs/Node生产环境3.jpg)

将复制的路径粘贴到命令行中，安装nvm
![](/aliyunnodejs/imgs/Node生产环境4.jpg)

安装指定的Node版本 v6.9.5，这里安装时间较长
![](/aliyunnodejs/imgs/Node生产环境5.jpg)

指定服务器使用该版本
nvm use v6.9.5 (指定使用版本)
nvm alias default v6.9.5 (指定默认版本)
![](/aliyunnodejs/imgs/Node生产环境6.jpg)

安装npm（taobao源）
npm --registry=https://registry.npm.taobao.org install -g npm

安装完npm以后需要执行以下操作用来指定监控数目
echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p
![](/aliyunnodejs/imgs/Node生产环境7.jpg)

安装Node所需的包(主要是pm2)
npm --registry=https://registry.npm.taobao.org install -g pm2 webpack gulp grunt-cli -g
![](/aliyunnodejs/imgs/Node生产环境8.jpg)

用户根路径下创建一个app.js文件:
sudo vi app.js

app.js复制粘贴以下内容
```
const http = require('http')

http.createServer(function(req, res){
res.writeHead(200,{'Content-Type': 'text/plain'})
res.end('There is a fire starting in my heart...')
}).listen(8082)

console.log('server running on http://112.74.179.8.:8082/')
```

保存退出后执行  node app.js

![](/aliyunnodejs/imgs/Node生产环境1-1.jpg)

在浏览器输入地址，但出现问题！！！
原来在于我们设置防火墙的时候没有给8082端口权限！
![](/aliyunnodejs/imgs/Node生产环境10.jpg)

重开一个窗口，编辑iptables配置文件
![](/aliyunnodejs/imgs/Node生产环境11.jpg)

退出保存后，重载配置
![](/aliyunnodejs/imgs/Node生产环境12.jpg)

刷新页面即可访问成功！
![](/aliyunnodejs/imgs/Node生产环境13.jpg)

**但是当我们按住ctrl+c退出node运行，或者关闭窗口时候再次访问网站则出现错误！**

原因在于，通过node app.js运行的生命周期有限，不能稳定地提供对外的Web服务

**神器：PM2**

pm2 start app.js // 运行app.js
现在我们就算关闭窗口，浏览器也可以直接访问得到网页啦
![](/aliyunnodejs/imgs/Node生产环境14.jpg)

刷新页面！
![](/aliyunnodejs/imgs/Node生产环境13.jpg)

pm2 list    // 这台服务器上正在运行的素有Node服务
pm2 show app // 查看正在
![](/aliyunnodejs/imgs/Node生产环境15.jpg)

pm2 logs
![](/aliyunnodejs/imgs/Node生产环境16.jpg)


```
静态站点需要满足以下两个条件:

1 稳定提供对外的web服务  -> pm2
2 通过外网在80端口访问而不是这里的8082端口  - Nginx
``` 
