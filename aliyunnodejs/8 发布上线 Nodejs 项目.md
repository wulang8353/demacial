# NodeJS项目线上部署

## 向发布上线 Nodejs 项目

* 创建代码托管仓库

创建私有仓库，这里推荐[码云](http://git.oschina.net/)

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
git merge origin/master --allow-unrelated-histories // 合并所有文件
```

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目8.jpg)

回车后会到以下的界面， 退出保存即可
![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目7.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目9.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目10.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目11.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目12.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目13.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目14.jpg)

![](/aliyunnodejs/imgs/向发布上线 Nodejs 项目15.jpg)
