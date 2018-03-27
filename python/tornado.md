#### Tornado架构图

![](/assets/tornado.png)

---

#### Tornado模块分类

    1. Core web framework

* tornado.web — 包含web框架的大部分主要功能，包含RequestHandler和Application两个重要的类
* tornado.httpserver — 一个无阻塞HTTP服务器的实现
* tornado.template — 模版系统
* tornado.escape — HTML,JSON,URLs等的编码解码和一些字符串操作
* tornado.locale — 国际化支持

2. Asynchronous networking 底层模块

* tornado.ioloop — 核心的I/O循环
* tornado.iostream — 对非阻塞式的 socket 的简单封装，以方便常用读写操作
* tornado.httpclient — 一个无阻塞的HTTP服务器实现
* tornado.netutil — 一些网络应用的实现，主要实现TCPServer类

3. Integration with other services

* tornado.auth — 使用OpenId和OAuth进行第三方登录
* tornado.database — 简单的MySQL服务端封装
* tornado.platform.twisted — 在Tornado上运行为Twisted实现的代码
* tornado.websocket — 实现和浏览器的双向通信
* tornado.wsgi — 与其他python网络框架/服务器的相互操作

4. Utilities

* tornado.autoreload — 生产环境中自动检查代码更新
* tornado.gen — 一个基于生成器的接口，使用该模块保证代码异步运行
* tornado.httputil — 分析HTTP请求内容
* tornado.options — 解析终端参数
* tornado.process — 多进程实现的封装
* tornado.stack\_context — 用于异步环境中对回调函数的上下文保存、异常处理
* tornado.testing — 单元测试



