# NodeJS项目线上部署

## 配置Nginx实现反向代理

首先确定服务器内未安装apache2，执行移除操作后更新包列表
![](/aliyunnodejs/imgs/Nginx反向代理1.jpg)

安装Nginx
![](/aliyunnodejs/imgs/Nginx 反向代理2.jpg)

进入到Nginx目录下，进入到conf.d中
ls 查看文件夹中的文件
这里有我原来创建的一个3000.conf不需要管它
新建另外一个配置文件conf
命名规则： <项目名>-com-<port>.conf
![](/aliyunnodejs/imgs/Nginx 反向代理6.jpg)

将以下代码输入进去

```
upstream umac {
    server 127.0.0.1:8082; // 这里就是刚才pm2下配置的端口
}

server {
    listen 80;
    server_name 112.74.179.8; // 自己的ip地址
    
    location / {
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-Nginx-Proxy true;
        
        proxy_pass http://umac;
        proxy_redirect off;
    }
}
```
最终效果如下图:
![](/aliyunnodejs/imgs/Nginx 反向代理1-1.jpg)

通过 sudo nginx -t 检测配置文件是否出错
![](/aliyunnodejs/imgs/Nginx 反向代理8.jpg)

检查正确后，重启Nginx
![](/aliyunnodejs/imgs/Nginx 反向代理9.jpg)

**这时候打开浏览器直接访问ip地址的80端口就可以得到页面了**
![](/aliyunnodejs/imgs/Nginx 反向代理10.jpg)

进入到上一层路径、如果所示，然后进入到nginx.conf修改配置
![](/aliyunnodejs/imgs/Nginx 反向代理13.jpg)

往下可以找到http中的server_tokens，改为off，退出保存
(隐藏nginx在响应头中的详细信息)
![](/aliyunnodejs/imgs/Nginx 反向代理12.jpg)

sudo service nginx reload重载nginx
![](/aliyunnodejs/imgs/Nginx 反向代理14.jpg)

打开浏览器，找到112.74.179.8(ip地址)的请求信息，在响应头重可以看到nginx
![](/aliyunnodejs/imgs/Nginx 反向代理11.jpg)







