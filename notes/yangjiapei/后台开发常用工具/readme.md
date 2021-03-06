# 后台开发需要准备的工具和常见问题

## 准备的工具

1. [wampServer](http://www.wampserver.com/en/) Apache Web服务器、PHP解释器以及MySQL数据库的整合软件包，免去了将时间花费在繁琐的配置环境过程，从而腾出更多精力去做开发。在windows下将Apache+PHP+Mysql 集成环境，拥有简单的图形和菜单安装和配置环境。

2. [mysql](https://dev.mysql.com/downloads/mysql/) 在本地安装mysql数据库

   - [mac安装mysql注意的问题](http://www.jianshu.com/p/fd3aae701db9)
   - [windows下安装mysql](http://www.cnblogs.com/lmh2072005/p/5656392.html)

3. [Navicat Premium](https://www.navicat.com.cn/download) 是一套数据库管理工具，以单一程序同時连接到 MySQL、MariaDB、SQL Server、SQLite、Oracle 和 PostgreSQL 数据库。

   - [download for mac](http://xclient.info/s/navicat-premium.html?_=2e3a142be5cabd704a705c52cf83125b)
   - [Navicat for mac 使用教程](http://www.jianshu.com/p/326c1aaa1052)

4. [Free SFTP Client for Windows :: WinSCP](https://winscp.net/eng/docs/lang:chs) WinSCP 是一个 Windows 环境下使用的 SSH 的开源图形化 SFTP 客户端。同时支持 SCP 协议。它的主要功能是在本地与远程计算机间安全地复制文件，并且可以直接编辑文件

   - [Mac Yummy FTP 1.11.14 快速安全的 FTP/SFTP/FTPS客户端](http://xclient.info/s/yummy-ftp.html?_=2e3a142be5cabd704a705c52cf83125b)

5. [PuTTY - Free SSH, Telnet, SFTP Client for Windows](https://www.ssh.com/ssh/putty/download) 使用PuTTY 从 Windows 连接到 Linux 。
   - ​Mac 环境 可以直接使用 terminal 中的 ssh
   ``` 
    // ssh kk@192.168.2.79
    ssh user@host
   ```

6. [Postman](https://chrome.google.com/webstore/detail/postman/fhbjgbiflinjbdggehcddcbncdddomop?hl=zh-CN) 接口调试工具，能很好的模拟发送各种请求，方便查看响应结果

## 知识点

1. [Express](https://github.com/expressjs/express) Node.js Web 应用框架

2. [Koa](https://github.com/koajs/koa)  koa 是由 Express 原班人马打造的，致力于成为一个更小、更富有表现力、更健壮的 Web 框架。可以免除重复繁琐的回调函数嵌套，并极大地提升错误处理的效率。koa 不在内核方法中绑定任何中间件，它仅仅提供了一个轻量优雅的函数库，使得编写 Web 应用变得得心应手。**注意**，使用 koa2, Nodejs版本必须 >= 7.6.0，事先学习 es6 的 [async](http://es6.ruanyifeng.com/#docs/async) 函数 

   - [Koa2 实用入门 - CNode技术社区](https://cnodejs.org/topic/5709959abc564eaf3c6a48c8)
   - [koa2进阶学习笔记](https://chenshenhai.github.io/koa2-note/)
   - [koa 技术分享](https://cnodejs.org/topic/56936889c2289f51658f0926)
   - [深入浅出koa](https://github.com/berwin/Blog/issues/8)
   - [koa example](https://github.com/berwin/Blog/issues/8)

3. [Sequelize](http://docs.sequelizejs.com/) Nodejs ORM 框架，操作数据库。
   - ​[Sequelize 中文API文档 - 1、快速入门、Sequelize类](https://itbilu.com/nodejs/npm/VkYIaRPz-.html)
   - [Sequelize 中文API文档 - 2、Model定义 Model类的API](https://itbilu.com/nodejs/npm/V1PExztfb.html)
   - [Sequelize 中文API文档 - 3、模型之间的关系/关联](https://itbilu.com/nodejs/npm/41qaV3czb.html)
   - [Sequelize 中文API文档 - 4、查询与原始查询](https://itbilu.com/nodejs/npm/VJIR1CjMb.html)
   - [Sequelize 中文API文档 - 5、实例的使用、Instance类介绍](https://itbilu.com/nodejs/npm/N1sdaHTzb.html)

   **注意**
   一、使用 findOne、findAll、findAndCountAll 查询的时候，得到的其实是一个Instance 对象，那如何得到 JSON 对象
   ```js
    Instance {
      dataValues: 
       { 
          id: 1,
          userID: '365',
          userName: '张三'
       },
      _previousDataValues: {}
    }
   ```

    在使用这些方法的时候使用`raw`, 增加 raw 选项后，会返回数据库中的原始结果
    ```js
   models.user.findOne({
     // 添加 raw
     raw: true,
     where: {}
   })
    ```

    二、使用关联查询 一对一、一对多、多对多 关系

    1、一对一(one-to-one)关联 -> BelongsTo 属于 、hasOne 拥有一个
    ```js
        var User = this.sequelize.define('user', {/* attributes */});
        var Company  = this.sequelize.define('company', {/* attributes */});

        User.belongsTo(Company); // 会为user 添加 companyId 属性
    ```
    2、一对多(One-To-Many)关联 -> hasMany 
    3、多对多(Belongs-To-Many)关联 -> belongsToMany

    三、mysql 数据库 存储表情

    在做工作圈项目遇到，在文本输入emoji进行存储的时候导致存储失败的问题，经本地调试发现emoji表情在存储时转成的四个字节（\xF0\x9F\x90\xAC）导致sequelize报错，Unhandled rejection SequelizeDatabaseError。由于数据库使用的是utf8字符集utf8_general_ci，这个校对规则（collation）最大只支持3个字节，所以四个字节的emoji就抛异常了。

    解决方法：使用utf8mb4字符集。它在与utf8数据格式处理性能相同基础上加强了对字符码位（code point）的处理能力。

    修改数据库编码为utf8mb4，更新sequelize的配置
    ```js
       1、 // 修改 options
        new Sequelize(config.db.database,
          config.db.username,
          config.db.password, {
            host: config.db.host,
            dialect: config.db.dialect,
            dialectOptions: {
              charset: 'utf8mb4'
            },
            pool: {
              max: config.db.pool.max,
              min: config.db.pool.min,
              idle: config.db.pool.idle
            },
            timezone: '+08:00'
          }
        )

        2、在模块定义的时候
        Sequelize.define('user', {
            userID: {
                type: DataTypes.INTEGER
            }
        }, {
            // 添加这两项参数
            charset: 'utf8mb4',
            collate: 'utf8mb4_bin',
        })
    ```

    关于sequelize相关配置issue可参看
    [https://github.com/sequelize/sequelize/issues/1220](https://github.com/sequelize/sequelize/issues/1220)
   ​    

   ​

   ​

   ​
