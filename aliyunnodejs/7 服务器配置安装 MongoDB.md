# NodeJS项目线上部署

**一般情况下，数据库和应用程序应该在2个不同的服务器中。为了节约成本，这里将二者均装在同一台服务器中**

## 服务器配置安装 MongoDB

[MongoDB官方安装教程](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-ubuntu/)

第一步：登陆到服务器，将命令直接粘贴到命令行中
![](/aliyunnodejs/imgs/服务器配置安装MongoDB1.jpg)

第二步：根据服务器Ubuntu版本进行选择，这里选择的14.04版本
![](/aliyunnodejs/imgs/服务器配置安装MongoDB2.jpg)

第三步： 更新本地包
![](/aliyunnodejs/imgs/服务器配置安装MongoDB3.jpg)

第四步： 安装Mongodb依赖包(需要一定时间)
![](/aliyunnodejs/imgs/服务器配置安装MongoDB4.jpg)

## 服务器开启MongoDB

开启Mongod服务

由于防火墙的设置，未允许本地连接27017这个端口，所以需要在iptables中进行修改