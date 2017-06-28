## 核心技术
```
NodeJS 核心开发语言
Express 基于Node.js Web应用框架
Mongodb 数据库

第三方模块 & 中间件
bodyParser：解析post请求数据(已经与express解绑)
cookies：   读/写cookies
swig：      模板解析引擎
mongoose：  操作mongodb数据
markdown：  语法解析生成模块
```

## 源码说明
### 项目目录说明
```
.
|-- db                               // 数据库存储目录
|-- models                           // 数据库模型文件目录
|-- node_modules                     // 启动，打包，部署，自动化构建
|-- public                           // 程序打包配置
|-- routers                          // 代理服务器配置
|-- schemas                          // 代理服务器配置
|-- views                            // 代理服务器配置
|-- app.js                           // 入口文件
|-- README.md                        // 项目说明
|-- package.json                     // 配置项目相关信息，通过执行 npm init 命令创建
.
```