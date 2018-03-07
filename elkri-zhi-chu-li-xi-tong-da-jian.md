### ELK日志服务部署

ELK ,全名Elasticsearch, Logstash, Kibana ,是一套日志实时处理系统。官网：[https://www.elastic.co/webinars/introduction-elk-stack](https://www.elastic.co/webinars/introduction-elk-stack), 以下是基于 ELK v6.2.2 + redis + centos6.x + jdk1.8   搭建的日志处理服务.

###### 安装JDK

   ........



##### 安装Logstash6.2.2

     此处Logstash 有两个实例，一个为Shipper,一个Indexer。 Shipper用来处理日志文件然后输出到Redis，Indexer用来从Redis取出日志发送给ElasticSearch存储。

          wget 

