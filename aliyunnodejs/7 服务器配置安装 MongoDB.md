# NodeJS项目线上部署

**一般情况下，数据库和应用程序应该在2个不同的服务器中。为了节约成本，这里将二者均装在同一台服务器中**

```
rm 删除文件
rm -rf 删除目录
```


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

-h：
MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017

-d：
需要备份的数据库实例，例如：test

-o：
备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。
```

![](/aliyunnodejs/imgs/服务器连接MongoDB2.jpg)

导出数据库后，存放在mongDB/bin中

![](/aliyunnodejs/imgs/服务器连接MongoDB3.jpg)

进入到express-demo文件中，可以看到导出的所以数据
![](/aliyunnodejs/imgs/服务器连接MongoDB4.jpg)

```
tar zcvf express-demo-backup.tar.gz express-demo-backup
tar zcvf: 打包文件
express-demo-backup.tar.gz： 打包后文件名
express-demo-backup： 指定打包当前路径下的哪一个文件
```
![](/aliyunnodejs/imgs/服务器连接MongoDB5.jpg)

打包文件存放在mongDB/bin中
![](/aliyunnodejs/imgs/服务器连接MongoDB6.jpg)

进入到服务器中，在根路径下新建一个dbbackup文件夹，专门用来存放数据库
![](/aliyunnodejs/imgs/服务器连接MongoDB7.jpg)

在根路径下通过ls可以看到已经创建dbbackup文件夹
![](/aliyunnodejs/imgs/服务器连接MongoDB8.jpg)

回到本地命令行，将本地数据库上传到服务器指定路径中

```
scp -P 39999 ./express-demo-backup.tar.gz umac@112.74.179.8:/home/umac/dbbackup/
scp -P 39999 ./express-demo-backup.tar.gz 上传打包文件至指定ip地址的39999端口
/home/umac/dbbackup/： 目标路径为根路径下的dbbackup文件夹中
```
![](/aliyunnodejs/imgs/服务器连接MongoDB9.jpg)


![](/aliyunnodejs/imgs/服务器连接MongoDB10.jpg)

```
mongorestore --host 127.0.0.1:19999 -d express-demo ./dbbackup/express-demo-backup/express-demo/

mongorestore --host 127.0.0.1:19999: 像服务器数据库的19999端口中存入数据
-d express-demo： 存入数据到 文件名为express-demo的库中
/dbbackup/express-demo-backup/express-demo/ 指定存放数据的路径

```
![](/aliyunnodejs/imgs/服务器连接MongoDB11.jpg)

操作成功后，登陆到端口号为19999的数据库中
![](/aliyunnodejs/imgs/服务器连接MongoDB12.jpg)

服务器端数据库中在movies表中的数据
![](/aliyunnodejs/imgs/服务器连接MongoDB13.jpg)

本地数据库中在movies表中的数据
![](/aliyunnodejs/imgs/服务器连接MongoDB10.jpg)

## 向服务器的已有数据库导入一个或者多个单表

