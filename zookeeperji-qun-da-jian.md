#### Zookeeper

zookeeper是apache项目下的开源分布式应用程序协调服务。用来确保集群中只有一个master。zookeeper使用java语言开发，运行需要安装jdk环境。

#### zookeeper部署

zookeeper集群运行至少需要两台机器，以下将使用三台机器搭建zookeeper集群，分别为192.168.10.193,192.168.10.194,192.168.10.195

三台机器的zookeeper配置文件均一样，所以只要安装一台机器，然后将程序拷贝至其他机器即可\(除myid文件外\)。

##### zookeeper安装

```
   1.  yum install java -y

   2.  wget http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz

   3.  tar -zxf zookeeper-3.4.10.tar.gz && mv zookeeper-3.4.10 /usr/local/zookeeper

   4.  vim /usr/local/zookeeper/config/zoo.cfg

          tickTime=2000
          initLimit=10
          syncLimit=5
          dataDir=/usr/local/zookeeper/data
          clientPort=2181
          server.1=192.168.10.193:2888:3888  #对应相应的id，如service.1 此处的1为id
          server.2=192.168.10.194:2888:3888
          server.3=192.168.10.195:2888:3888
   5. mkdir /usr/local/zookeeper/data && echo 1 > /usr/local/zookeeper/data/myid  #
   
   6.  scp -r /usr/local/zookeeper 192.168.10.194:/usr/local/  #登录194 修改/usr/local/zookeeper内容为2
   
   7. scp -r /usr/local/zookeeper 192.168.10.195:/usr/local/  #登录195 修改/usr/local/zookeeper内容为 3

```



