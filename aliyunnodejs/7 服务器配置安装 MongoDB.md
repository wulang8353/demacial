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

## 服务器连接MongoDB

通过sudp service mongod start 开启Mongod服务即可
![](/aliyunnodejs/imgs/服务器配置安装MongoDB5.0.jpg)

数据库默认端口是27017，为提高安全起见，修改默认端口
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.0.jpg)

向下找到net对象中的port,这里默认端口修改成19999
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.1.jpg)

退出保存后重启服务
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.2.0.jpg)

由于防火墙的设置，未允许本地连接27017这个端口，所以需要在iptables中进行修改
通过sudo vi /etc/iptables/up.rules 进入到配置文件修改，然后退出保存
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.2.jpg)

修改内容如 mongodb connect中所示
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.3.jpg)

保存后，重载防火墙配置文件
![](/aliyunnodejs/imgs/服务器配置安装MongoDB5.1.jpg)

因为修改了默认端口，所以 通过  mongo --port 19999 连接数据库
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.5.jpg)

图下表示成功连接MongoDB数据库
![](/aliyunnodejs/imgs/服务器配置安装MongoDB6.6.jpg)

## 向服务器导入完整数据

进入到mongDB/bin中(要在本地电脑上启动Mongodb的服务哦)

![](/aliyunnodejs/imgs/服务器连接MongoDB1.jpg)

```
在这里执行git bash

./mongodump -h 127.0.0.1:27017 -d express-demo -o express-demo-backup
./mongodump：表示执行当前路径下的mongodump程序，用来导出数据库
-h 127.0.0.1:27017 ： 指定本地上数据库默认27017端口
-d express-demo： 指定在27017端口中数据库名为express-demo的数据
-o express-demo-backup 将数据存放在express-demo-backup这个文件名的文件中
```

![](/aliyunnodejs/imgs/服务器连接MongoDB2.jpg)

导出数据库后，存放在mongDB/bin中

![](/aliyunnodejs/imgs/服务器连接MongoDB3.jpg)

进入到express-demo文件中，可以看到导出的所以数据
![](/aliyunnodejs/imgs/服务器连接MongoDB4.jpg)

![](/aliyunnodejs/imgs/服务器连接MongoDB5.jpg)

![](/aliyunnodejs/imgs/服务器连接MongoDB6.jpg)

![](/aliyunnodejs/imgs/服务器连接MongoDB7.jpg)

![](/aliyunnodejs/imgs/服务器连接MongoDB8.jpg)

![](/aliyunnodejs/imgs/服务器连接MongoDB9.jpg)




