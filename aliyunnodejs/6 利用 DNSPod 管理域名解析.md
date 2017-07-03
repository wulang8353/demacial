# NodeJS项目线上部署

**域名没有备案成功前，不要进行域名解析操作**

概念：
* 域名需要一个域名解析器，其作用是为了把域名解析到某个IP地址
      这样就可以直接通过域名访问到对应的服务器
* 一个域名只可以对应一个IP地址，而一个IP地址可以对应多个域名
      如果是在虚拟空间，就会出现和其他网站是在同一台服务器上，公用着同一个IP地址
      这意味着存在着这样一种情况，当同一台服务器的其他的网站因为不良信息被降权，则自己的网站受到牵连而一样被降级

## 利用 DNSPod [DNSPod](https://www.dnspod.cn/) 管理域名解析



进入到阿里云的控制台，找到域名
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析1.jpg)

点击通过备案的域名尾部的管理
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析2.jpg)

点击DNS修改后，点击右边的修改域名DNS
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析3.jpg)

将下图所示的两段非万网DNS（这里是DNSPod）填入到下方两栏中，点击确认
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析4.jpg)

确认后，提示修改成功，一般会在2小时内生效
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析6.jpg)

进入到[DNSPod](https://www.dnspod.cn/)网站，创建账号后，进入到控制条
![](/aliyunnodejs/imgs/利用 DNSPod 管理域名解析5.jpg)

