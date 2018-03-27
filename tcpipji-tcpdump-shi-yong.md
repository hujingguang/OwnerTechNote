#### IP协议头分析图

![](/assets/ip.png)

---

#### TCP协议头分析图

![](/assets/tcp.png)

---

#### tcpdump命令使用

```
   type字段：修饰符指定id 所代表的对象类型

      可选的对象类型有: host, net, port 以及portrange  如, 'host foo', 'net 128.3', 'port 20', 'portrange 6000-6008' . 
      如果不指定type 修饰符, id默认的修饰符为host.


   dir字段：修饰符描述id 所对应的传输方向

      即发往id 还是从id 接收.可取的方向为: src, dst, src 或 dst, src并且dst. 
      例如, 'src foo','dst net 128.3', 'src or dst port ftp-data'.(nt: 分别表示符合条件的数据包中, 
      源主机是foo, 目的网络是128.3, 源或目的端口为 ftp-data).
      如果不指定dir修饰符, id 默认的修饰符为src 或 dst.对于链路层的协议,比如SLIP(nt: Serial Line InternetProtocol,
       串联线路网际网络协议), 以及linux下指定'any' 设备, 并指定'cooked'(nt | rt: cooked 含义未知, 需补充) 抓取类型, 
       或其他设备类型,可以用'inbound' 和 'outbount' 修饰符来指定想要的传输方向.

    proto字段: 修饰符描述id 所属的协议
       可选的协议有: ether, fddi, tr, wlan, ip, ip6, arp, rarp, decnet, tcp以及 upd.
```



