#### Zookeeper

zookeeper是apache项目下的开源分布式应用程序协调服务。用来确保集群中只有一个master。zookeeper使用java语言开发，运行需要安装jdk环境。

#### zookeeper部署

zookeeper集群运行至少需要两台机器，以下将使用三台机器搭建zookeeper集群，分别为192.168.10.193,192.168.10.194,192.168.10.195

三台机器的zookeeper配置文件均一样，所以只要安装一台机器，然后将程序拷贝至其他机器即可\(除myid文件外\)。

1.  获取源代码

         wget  http://mirror.bit.edu.cn/apache/zookeeper/zookeeper-3.4.10/zookeeper-3.4.10.tar.gz

   2.  安装jdk

         yum  install java -y

