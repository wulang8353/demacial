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



![](/aliyunnodejs/imgs/Node生产环境9.jpg)

![](/aliyunnodejs/imgs/Node生产环境10.jpg)

![](/aliyunnodejs/imgs/Node生产环境11.jpg)

![](/aliyunnodejs/imgs/Node生产环境12.jpg)

![](/aliyunnodejs/imgs/Node生产环境13.jpg)

![](/aliyunnodejs/imgs/Node生产环境14.jpg)

![](/aliyunnodejs/imgs/Node生产环境15.jpg)

![](/aliyunnodejs/imgs/Node生产环境16.jpg)


  
  
 

const http = require('http')

http.createServer(function(req, res){
  res.writeHead(200,{'Content-Type': 'text/plain'})
  res.end('我就是力量的化身！')
}).listen(80)

console.log('server running on http://112.74.179.8/')


## 静态站点

静态站点稳定提供对外的web服务


通过外网在80端口访问