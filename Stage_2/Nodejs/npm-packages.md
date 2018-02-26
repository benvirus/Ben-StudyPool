# 常用NPM包讲解

### 目录

<!-- MarkdownTOC -->

- Express
- passport
- bcrypt
- multer
- pm2
- mysql

<!-- /MarkdownTOC -->


### Express

Express 是Nodejs的一个服务端应用MVC框架。Express的核心思想就是把一个完整的HTTP请求可以看作是从请求到响应中间经过大量中间件处理的结果。所以在Express中的核心就是中间件。基于此，NPM中出现了大量的为Express开发的中间件。

### passport

简单的理解就是通行证，既然是通行证，那么就涉及到身份认证。所以它最基本的是可以拿来做身份认证的。比如用户的本地注册，登陆认证。更多的，比如第三方认证：Google,Facebook等。。。具体用法可以参考官方文档。

[官方文档](http://passportjs.org/docs/overview)

### bcrypt

bcrypt 是现在业内比较认同的数据库密码保存方案的实施工具。众所周知，为了防止服务器被拖库之后不会完全的一丝不挂，密码不会明文保存在数据库中，最基本的也会采用MD5加密一下。但是MD5变的越来越不安全，可以被解密。现在最推荐的做法是对密码进行HASH加盐。做法就是先对明文的密码进行HASH计算，再对结果进行加盐处理，然后把结果保存在数据库中。bcrypt 分别提供了同步和异步的hash计算和加盐的API，同时也提供了明文密码和数据库中hash加盐后的结果进行验证的API，可谓好用至极啊。

[官方文档](https://www.npmjs.com/package/bcrypt)

### multer

multer 就是Express非常常用的中间件之一。他的核心功能是帮你处理掉了文件上传的诸多接收存储等细节。你只需要关心在有文件上传的时候，直接从请求对象中获取上传文件的相关描述就好了，比如：文件存储地址，文件类型，文件大小，文件名称等等，，然后进行后续的一系列的文件处理。

### pm2

这是一个node应用进程管理工具。可以批量的管理你的多个Node应用。

[官方文档](http://pm2.keymetrics.io/)

### mysql

Nodejs 的 mysql数据库接口，提供给你使用Nodejs来操作数据库，执行数据库命令的API。

[官方文档](https://www.npmjs.com/package/mysql)