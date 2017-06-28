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
|   |-- Category                     // 种类
|   |-- Content                      // 内容
|   |-- User                         // 用户
|-- node_modules                     // 依赖包
|-- public                           // 静态文件
|   |-- css                          // 各种css文件
|   |-- fontAwersome                 // 字体文件
|   |-- font                         // 字体文件
|   |-- imgs                         // 各种图片
|   |-- js                           // 各种js文件
|-- routers                          // 代理服务器配置
|   |-- admin                        // 管理员路由以及功能逻辑
|   |-- api                          // 通用逻辑
|   |-- main                         // 前端路由以及功能逻辑
|-- schemas                          // 代理服务器配置
|   |-- categories                   // 分类的表结构
|   |-- Contents                     // 内容的表结构
|   |-- users                        // 用户的表结构
|-- views                            // 页面组件
|   |-- admain                       // 管理员页面
|   |-- main                         // 主页页面
|-- .gitnore                         // git配置文件
|-- app.js                           // 入口文件
|-- README.md                        // 项目说明
|-- package.json                     // 配置项目相关信息
.
```