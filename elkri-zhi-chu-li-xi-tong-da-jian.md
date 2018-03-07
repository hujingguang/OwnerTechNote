### ELK日志服务部署

ELK ,全名Elasticsearch, Logstash, Kibana ,是一套日志实时处理系统。官网：[https://www.elastic.co/webinars/introduction-elk-stack](https://www.elastic.co/webinars/introduction-elk-stack), 以下是基于 ELK v6.2.2 + redis + centos6.x + jdk1.8 +x-pack插件  搭建的日志处理服务.

###### 安装JDK

........

安装Redis

```
yum install redis -y   #
```

##### 安装Logstash6.2.2

此处Logstash 有两个实例，一个为Shipper,一个Indexer。 Shipper用来处理日志文件然后输出到Redis，Indexer用来从Redis取出日志发送给ElasticSearch存储。

```
  1.  wget   https://artifacts.elastic.co/downloads/logstash/logstash-6.2.2.zip

  2.  unzip logstash-6.2.2.zip && cp -a logstash-6.2.2 /usr/local/logstash-shipper  # shipper 实例

  3.   cp -a logstash-6.2.2 /usr/local/logstash-indexer  #indexer 实例


  #配置logstash-shipper 的 log.conf

  4.  vim  /usr/local/logstash-shipper/log.conf

      input{ 
              file{
               path => ["/root/access.log"]   
               start_position => "beginning"
                   }
            }

     filter{ 
       grok{
        match => {
         "message" => "%{IP:remote_ip} - %{DATA:remote_user} \[%{HTTPDATE:visit_date}\] \"%{WORD:method} %{URIPATH:uri} %{DATA:protocol}\" %{NUMBER:status} %{NUMBER:send_byte} \"%{DATA:referer}\" \"%{DATA:user_agent}\" \"%{DATA:forward_ip}\""
      }    #日志格式正则匹配，根据自己的日志格式进行修改,我的日志格式为： '$remote_addr - $remote_user [$time_local] "$request"  '$status $body_bytes_sent "$http_referer" ' '"$http_user_agent" "$http_x_forwarded_for"';
       remove_field => ["message","@version","host"]
       add_field => {
            "host_name" => "ld-vm-201"  #主机名,用来标记哪台服务器的日志
          } 
              }
       }

     output{
       if "_grokparsefailure" not in [tags]{
                 redis{
                    data_type => "channel"
                    key => "nginx-chan"
                    host => "127.0.0.1"
                    port => 6379
                 }                         #输出到Redis
                stdout{
                  codec => rubydebug       #同时输出到控制台查看,生产环境可去掉输出到控制台
               }
      }
    }
  5. cd /usr/local/logstash-shipper && ./bin/logstash-plugin install logstash-input-redis 
   
  6. cd /usr/local/logstash-shipper && ./bin/logstash-plugin install logstash-patterns-core
  
  7. cd /usr/local/logstash-indexer && ./bin/logstash-plugin install logstash-input-redis 
  
  8. /usr/local/logstash-shipper/bin/logstash -f /usr/local/logstash-shipper/log.conf  #启动服务

  #配置logstash-indexer

  9. vim /usr/local/logstash-indexer/redis.conf

      input{
        redis{
       data_type => "pattern_channel"
       key => "nginx-chan"  
       host => "127.0.0.1"
       port => 6379
             }
       }

       output{
          elasticsearch {
         hosts => ["127.0.0.1:9200"]    #elasticsearch 监听地址端口
         index => "logstash-nginx-chan-%{+yyyy.MM.dd}"
         template_overwrite => true
         manage_template => false
         user => "elastic"                #elasticsearch 用户密码，待会安装elasticsearch时生成再填写
         password => "123123"
         codec => json
         }
    stdout{
     codec => rubydebug
   }

}

  10. vim  /usr/local/logstash-indexer/config/logstash.yml

      xpack.monitoring.elasticsearch.url: "http://127.0.0.1:9200"
      xpack.monitoring.elasticsearch.username: "logstash_system"    #安装elasticsearch时生成再填写
      xpack.monitoring.elasticsearch.password: "123123"

  11. cd /usr/local/logstash-indexer  && ./bin/logstash-plugin install x-pack #安装x-pack插件
  
  
```

##### 安装Elasticsearch6.2.2

```
 1.  wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.2.zip
 
 2.  unzip elasticsearch-6.2.2.zip && cp -a elasticsearch-6.2.2  /usr/local/elasticsearch
 
 3. mkdir /usr/local/elasticsearch/data
 
 4. useradd elastic && chown -R elastic.elastic /usr/local/elasticsearch
 
 5. vim /usr/local/elasticsearch/config/
 
 
```



