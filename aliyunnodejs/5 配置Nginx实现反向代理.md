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
    server 127.0.0.1:8082;
}

server {
    listen 80;
    server_name 112.74.179.8;
    
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
![](/aliyunnodejs/imgs/Nginx 反向代理1-1.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理3.jpg)



![](/aliyunnodejs/imgs/Nginx 反向代理5.jpg)

11
![](/aliyunnodejs/imgs/Nginx 反向代理7.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理8.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理9.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理10.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理11.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理12.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理13.jpg)

![](/aliyunnodejs/imgs/Nginx 反向代理14.jpg)

