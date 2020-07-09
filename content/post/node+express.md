---
title: 使用node+express搭建服务器
date: 2019-07-11
tags:
  - node
  - express
categories:
  - 后端
  - node
---

> 实验所用服务器ip地址：152.136.139.114

### 配置服务器环境

通过ssh登陆服务器，然后用apt安装nodejs和npm。经过这样简单的配置就可以把服务器环境弄好啦。

### 实现静态文件的访问

创建一个文件夹，用npm在其中创建一个项目。

```shell
$ npm install express --save
```

这样便可以在这个文件夹中安装express。然后我们创建一个`index.js`，便可以在里面用express了。

至于具体的实现，主要是依靠如下的代码。

```javascript
app.use(express.static(path.join(__dirname, '/public')))
```

这样便可以让实现对`/public`目录下静态文件的访问。

```javascript
app.listen(8000)
```

令其监听8000端口，便可以通过`http://152.136.139.114:8000/index.html`访问我的个人主页了。

### 实现接口

为`/api`设定一个router，我们在`router.js`中编写相应的接口。

首先要注意验证请求头中的`hw-token`，如果不匹配直接返回403。

在这个基础上就可以写这4个api了，注意POST请求类型为Content-Type='multipart/form-data'，所以需要我们引入`multer`。

然后正常编写即可。

> 服务器这几天一直都会运行这个进程。
>
> 静态页面：[](http://152.136.139.114:8000/index.html)

