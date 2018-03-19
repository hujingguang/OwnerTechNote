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
                    data_type => "list"
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
       data_type => "list"
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

 5. vim /usr/local/elasticsearch/config/elasticsearch.yml

     network.host: 0.0.0.0
     action.auto_create_index: true
     http.cors.enabled: true
     http.cors.allow-origin: "*"
     http.cors.allow-headers: "Authorization"
     xpack.security.audit.enabled: true
     bootstrap.memory_lock: false
     bootstrap.system_call_filter: false

  6. su - elastic && cd /usr/local/elasticsearch && ./bin/elasticsearch-plugin install x-pack

  7. vim /etc/profile  #添加如下内容

       ulimit -n 65536
       ulimit -u 4096

     vim /etc/sysctl.conf   #添加如下内容
        vm.max_map_count=655360

     执行sysctl -p 使其生效

  8. su - elastic && cd /usr/local/elasticsearch && ./bin/x-pack/setup-passwords interactive #此处生成logstash_system,elastic,kibana三个用户的密码，分别用来填写安装Logstash,Kibana时配置文件中的用户和密码

  9  su - elastic && cd /usr/local/elasticsearch && ./bin/elasticsearch   #启动服务，若无报错加 -d 选项作为守护进程启动
```

##### 安装Kibana6.2.2

```
  1. wget  https://artifacts.elastic.co/downloads/kibana/kibana-6.2.2-linux-x86_64.tar.gz

  2. tar -zxvf kibana-6.2.2-linux-x86_64.tar.gz && mv kibana-6.2.2-linux-x86_64 /usr/local/kibana

  3. cd /usr/local/kibana && ./bin/kibana-plugin install x-pack

  4.  vim /usr/local/kibana/config/kibana.yml

         server.host: "0.0.0.0"
         elasticsearch.url: "http://127.0.0.1:9200"
         elasticsearch.username: "kibana"
         elasticsearch.password: "123123"         #此处填写安装elasticsearch 时第八步生成的kibana用户名和密码
         logging.useUTC: false
  5.  cd /usr/local/kibana && ./bin/kibana   #启动服务
```

##### **激活X-pack**

```
    浏览器打开  https://register.elastic.co/xpack_register   获取basic license

    vim   t.sh    #将获取的license信息依次填入相应的key中。


    curl -XPOST 'http://elastic:123123@192.168.10.201:9200/_xpack/license?pretty&acknowledge=true' -H 'Content-Type: application/json' -d'
{
  "licenses": [
    {
      "uid":"8e7c12d3-3980-4167-aefa-73da676e06fb",
      "type":"basic",
      "start_date_in_millis": 1520985600000,
      "issue_date_in_millis":1520985600000,
      "expiry_date_in_millis":1552607999999,
      "max_nodes":100,
      "issued_to":"xxxxx",
      "issuer":"Web Form",
      "signature":"xxxxxxx"
    }
    ]
}
'
   bash  ./t.sh   
    #执行脚本，返回 
     {
   "acknowledged" : true,
    "license_status" : "valid"
     } 表示激活成功
```

##### 破解x-pack6.2.2

```
       下载破解版 x-pack-core.jar包和激活license脚本

       x-pack-core.jar 地址： https://pan.baidu.com/s/1k8q8U1_i1sgq9ZtjL2SyDQ

       license脚本地址： https://pan.baidu.com/s/1_sws0vovuDU6zloNTrjHQg


       将下载的jar包替换 /usr/local/elasticsearch/plugin/x-pack/x-pack-core/x-pack-core-6.2.2.jar

       修改 /usr/local/elasticsearch/config/elasticsearch.yml

          xpack.security.enabled: false  #修改为false

       重启 elasticsearch

       执行下载的license脚本(修改里面的地址为你自己的elastic监听地址)

       bash  input_license.sh
```

##### **ElasticSearch集群配置 + LDAP认证**

```
      用破解的x-pack-core-6.2.2.jar替换elastic集群中x-pack 未破解的jar,license不用在其他数据节点再次激活了

      cd /usr/local/elasticsearch/   && ./bin/x-pack/certgen   #生成公私密钥，需要连续生成所有节点的

      修改jdk

      vim /usr/local/jdk1.8/jre/lib/security/java.policy  添加一行
         permission java.io.FilePermission "<<ALL FILES>>", "read";

      Master  节点(192.168.10.201)配置文件如下：  
----------------------------------------------------------------------------------------------
network.host: 0.0.0.0
http.port: 9200
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
action.auto_create_index: true
http.cors.enabled: true
http.cors.allow-origin: "*"
http.cors.allow-headers: "Authorization"
xpack.security.audit.enabled: true
xpack.security.enabled: true
xpack.security.transport.ssl.enabled: true
cluster.name: my-application
node.name: node-201
node.master: true
node.data: true
discovery.zen.ping.unicast.hosts: ['192.168.10.201:9300']
node.max_local_storage_nodes: 2
xpack.ssl.key: /tmp/test/node201/node201.key
xpack.ssl.certificate: /tmp/test/node201/node201.crt
xpack.ssl.certificate_authorities: /tmp/test/ca/ca.crt
xpack.security.authc.realms:
  ldap1:
    type: ldap
    order: 0
    url: "ldap://ldldap.yourIP.com"
   # bind_dn: "cn=admin,dc=domain,dc=com"
   # bind_password: HSCJT3wtbv
    user_dn_templates:
      - "cn={0}, ou=users,dc=domain, dc=com"
    group_search:
      base_dn: "dc=domain,dc=com"
    files:
      role_mapping: "/usr/local/1-elasticsearch/config/mapping.yml"
    unmapped_groups_as_roles: false

-----------------------------------------------------------------------------------

    #/usr/local/1-elasticsearch/config/mapping.yml 配置如下：
        kibana_user:
          - "cn=hjg,ou=users,dc=laidiantech,dc=com"
        tomcat_log_role:
         - "cn=hjg,ou=users,dc=laidiantech,dc=com"



-----------------------------------------------------------------------------------------------

 Slave 数据节点(192.168.10.199)配置文件如下：  


cluster.name: my-application
node.name: node-199
node.master: false
node.data: true
network.host: 0.0.0.0
http.port: 9201
transport.tcp.port: 9301  
bootstrap.memory_lock: false
bootstrap.system_call_filter: false
action.auto_create_index: true
http.cors.enabled: true
http.cors.allow-origin: "*"
http.cors.allow-headers: "Authorization"
discovery.zen.ping.unicast.hosts: ['192.168.10.201:9300','192.168.10.199:9301']
xpack.security.enabled: true
node.max_local_storage_nodes: 2
xpack.ssl.key: /tmp/test/node199/node199.key
xpack.ssl.certificate: /tmp/test/node199/node199.crt
xpack.ssl.certificate_authorities: /tmp/test/ca/ca.crt
xpack.security.transport.ssl.enabled: true
```

###### **附： 多个日志文件处理 logstash -shipper 配置如下**

```
    input{ 
   file{
    path => "/data/cnt_cdt_api/logs/access.log"
    start_position => "beginning"
    type => 'cnt_cdt_api_access'
  }
  file{
      path => "/data/cnt_cdt_api/logs/error.log"
      type => 'cnt_cdt_api_error'
      codec => multiline{
            pattern => "^201"
            negate => true
            what => "previous"              #处理tomcat日志中包含多行的处理
            }
      start_position => "beginning"
    }
}
filter{ 
     if [type] == "cnt_cdt_api_access"{
       grok{
        match => {
          "message" => "%{MYDATE:log_date} %{DATA:number} \[%{LOGLEVEL:level}\] %{DATA:position} %{DATA:thread} %{GREEDYDATA:info}"
      }
       remove_field => ["@version","host","message"]
       add_field => {
            "host_name" => "ld-vm-247"
            "application" => "cnt_cdt_api"
          } 
     }
   }
    if [type] == "cnt_cdt_api_error"{
          grok{
            match => {
                   "message" => "%{MYDATE:log_date} %{DATA:number} \[%{LOGLEVEL:level}\] %{DATA:position} %{DATA:thread} %{GREEDYDATA:info}"
                     }
            remove_field => ["@version","host","message","tags"]
            add_field => {
                 "host_name" => "ld-vm-247"
                 "application" => "cnt_cdt_api"
                }
            }
       }
}

output{
       if "_grokparsefailure" not in [tags]{
         if [type] == "cnt_cdt_api_access"{
                 redis{
                    data_type => "list"
                    key => "cnt-cdt-api-access-chan"
                    host => "192.168.10.201"
                    port => 6379
                 }
                stdout{
                  codec => rubydebug
               }
            }
         if [type] == "cnt_cdt_api_error"{
                 redis{
                    data_type => "list"
                    key => "cnt-cdt-api-error-chan"
                    host => "192.168.10.201"
                    port => 6379
                 }
                stdout{
                  codec => rubydebug
               }
            }
      }
}
```

###### **多个日志文件处理 logstash -indexer配置如下**

```
input{
    redis{
       data_type => "list"
       key => "nginx-chan"
       host => "192.168.10.201"
       port => 6379
     }
    redis{
       data_type => "list"
       key => "cnt-cdt-api-access-chan"
       host => "192.168.10.201"
       port => 6379
     }
    redis{
       data_type => "list"
       key => "cnt-cdt-api-error-chan"
       host => "192.168.10.201"
       port => 6379
     }
}
output{
    if [type] == "cnt_cdt_api_access" {
      elasticsearch {
         hosts => ["127.0.0.1:9200"]
         index => "cnt-cdt-api-access-chan-%{+yyyy.MM.dd}"
         template_overwrite => true
         manage_template => false
         user => "elastic"
         password => "123123"
         codec => json
         }
    stdout{
     codec => rubydebug
   }
   }
  if [type] == "cnt_cdt_api_error" {
        elasticsearch {
         hosts => ["127.0.0.1:9200"]
         index => "cnt-cdt-api-error-chan-%{+yyyy.MM.dd}"
         template_overwrite => true
         manage_template => false
         user => "elastic"
         password => "123123"
         codec => json
         } 
         stdout{
               codec => rubydebug
   }
           }

}
```



