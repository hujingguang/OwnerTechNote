#### **Keepalived**

keepalived服务用来实现高可用,使用VRRP协议进行通信,当同一区域内master主机失效后,slave通过竞选成为新的master继续提供服务。

官方文档：[http://www.keepalived.org/doc/](http://www.keepalived.org/doc/)

keepavlied两个功能：1.使用VRRP协议进行failover，2.检查LVS system 的健康状态

![](/assets/Z@K606~D[D8WRFYLDP{6FMA.png)

###### 架构图如下

![](/assets/]P8ID5O50EG%28J[C[QN`[F18.png)

```
 # Configuration File for keepalived
global_defs {                         #全局定义模块
    notification_email {              #设置邮件通知用户
        admin@domain.com
        0633225522@domain.com
    }
    notification_email_from keepalived@domain.com  #设置发件人
    smtp_server 192.168.200.20              #邮件服务器地址
    smtp_connect_timeout 30              #邮件服务器连接超时时间
    lvs_id LVS_MAIN                      #
}
# VRRP Instances definitions
vrrp_sync_group VG1   #同步vrrp级，用于确定失败切换（FailOver）包含的路由实例个数。即在有2个负载均衡器的场景，一旦某个负载均衡器失效，需要自动切换到另外一个负载均衡器的实例是哪
    group {
        VI_1
        VI_2
    }
}
vrrp_sync_group VG2 {
    group {
        VI_3
        VI_4
    }
}
vrrp_instance VI_1 {
    state MASTER         #设置状态MASTER为主,提供服务
    interface eth0       #设置VIP绑定的网卡
    virtual_router_id 51  #设置集群的识别ID
    priority 150          #设置优先级，越大优先级越高
    advert_int 1          #设置广播时间间隔
    authentication {      #设置认证类型和密码
        auth_type PASS
        auth_pass k@l!ve1
    }
    virtual_ipaddress {   #虚拟IP组
        192.168.200.10
        192.168.200.11
    }
}
vrrp_instance VI_2 {
    state MASTER
    interface eth1
    virtual_router_id 52
    priority 150
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass k@l!ve2
    }
    virtual_ipaddress {
        192.168.100.10
    }
}
vrrp_instance VI_3 {
    state BACKUP
    interface eth0
    virtual_router_id 53
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass k@l!ve3
    }
    virtual_ipaddress {
        192.168.200.12
        192.168.200.13
    }
}
vrrp_instance VI_4 {
    state BACKUP
    interface eth1
    virtual_router_id 54
    priority 100
    advert_int 1
    authentication {
        auth_type PASS
        auth_pass k@l!ve4
    }
    virtual_ipaddress {
        192.168.100.11
    }
}
# Virtual Servers definitions
virtual_server 192.168.200.10 80 {   #虚拟服务器组配置
    delay_loop 30          #检查后端服务器的时间间隔
    lb_algo wrr            #负载均衡算法配置，使用wrr算法
    lb_kind NAT            #LVS负载均衡模式
    persistence_timeout 50  #持久化连接超时时间
    protocol TCP         #通信协议配置
    sorry_server 192.168.100.100 80  #后端服务器不可用时,错误页面服务器
    real_server 192.168.100.2 80 {    #后端服务器配置
        weight 2                     #权重配置
        HTTP_GET {                   #健康检查方式,使用http协议 
            url {
                path /testurl/test.jsp
                digest ec90a42b99ea9a2f5ecbe213ac9eba03
            }
            url {
                path /testurl2/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334c
            }
            connect_timeout 3          #健康检查请求时的超时时间
            retry 3                    #重试次数
            delay_before_retry 2    #重试间隔时间
        }
    }
    real_server 192.168.100.3 80 {
        weight 1
        HTTP_GET {
            url {
                path /testurl/test.jsp
                digest 640205b7b0fc66c1ea91c463fac6334c
            }
            connect_timeout 3
            retry 3
            delay_before_retry 2
        }
    }
}
virtual_server 192.168.200.12 443 {
    delay_loop 20
    lb_algo rr
    lb_kind NAT
    persistence_timeout 360
    protocol TCP
    real_server 192.168.100.2 443 {
        weight 1
        TCP_CHECK {
            connect_timeout 3
        }
    }
    real_server 192.168.100.3 443 {
        weight 1
        TCP_CHECK {
            connect_timeout 3
        }
    }
}
```



