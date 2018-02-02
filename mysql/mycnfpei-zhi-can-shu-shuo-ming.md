Mysql配置文件参数说明

```
         #系统参数。
         datadir=/usr/local/mysql/data     #数据库文件存放目录
         basedir=/usr/local/mysql  # Mysql安装目录
         socket=/usr/local/mysql/vars/mysql.sock    #socket文件路径
         port = 33060                  #监听端口
         bind-address=172.22.0.8       #监听地址
         server_id = 1                  #server id，标识mysql实例
         user=mysql                 #运行mysql服务的用户
         skip-name-resolve          #跳过ip地址解析成域名,加快客户端连接
         transaction_isolation = REPEATABLE-READ   #事务级别,
         max_connections=2000      #最大客户端连接数
         back_log=1200             #达到最大连接数时允许进入等待队列的连接,超过则直接拒绝客户端连接
         
         
         #日志配置
         log_bin=/usr/local/mysql/logs/mysql-bin   #二进制日志文件路径
         binlog_format=row          #二进制日志文件格式
         max_binlog_size = 2G       #二进制文件最大容量
         log_error=/usr/local/mysql/logs/error.log   #错误日志路径
         slow_query_log=1           #开启慢日志记录
         long_query_time=1          #慢日志记录时间,为1表示超过1s的查询记录到慢日志文件
         slow_query_log_file=/usr/local/mysql/logs/slow.log   #慢日志文件路径
         expire_logs_days=7         #日志文件过期时间,为7表示只保留七天内的日志文件
         

         
         
         
         
         
         
         
         
         
         
         
         
         
         
         
         
         
         
         
```



