#### **Keepalived**

keepalived服务用来实现高可用,使用VRRP协议进行通信,当同一区域内master主机失效后,slave通过竞选成为新的master继续提供服务。

官方文档：[http://www.keepalived.org/doc/](http://www.keepalived.org/doc/)

keepavlied两个功能：1.使用VRRP协议进行failover，2.检查LVS system 的健康状态

![](/assets/Z@K606~D[D8WRFYLDP{6FMA.png)

###### 架构图如下

![](/assets/]P8ID5O50EG%28J[C[QN`[F18.png)



      helloworld

