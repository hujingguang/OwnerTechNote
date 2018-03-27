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
         innodb_file_per_table = 1  #开启innodb存储引擎每个表单独文件存储
         default-storage-engine=InnoDB  #默认存储引擎为innodb


         #日志配置
         log_bin=/usr/local/mysql/logs/mysql-bin   #二进制日志文件路径
         binlog_format=row          #二进制日志文件格式
         max_binlog_size = 2G       #二进制文件最大容量
         log_error=/usr/local/mysql/logs/error.log   #错误日志路径
         slow_query_log=1           #开启慢日志记录
         long_query_time=1          #慢日志记录时间,为1表示超过1s的查询记录到慢日志文件
         slow_query_log_file=/usr/local/mysql/logs/slow.log   #慢日志文件路径
         expire_logs_days=7         #日志文件过期时间,为7表示只保留七天内的日志文件


         #主从配置参数
         log-slave-updates=1   #开启从relay_log同步数据的时候同时写入binlog中,用于ABC复制模型
         skip_slave_start=1    #关闭启动服务时自动启动start slave  从库配置此参数
         slave-parallel-workers=4  #设置主从复制时从服务器上的工作线程,加快主从复制,从库配置此参数
         master-verify-checksum=1  #主从进行二进制日志传输时确认文件checksum,确保数据传输正确性,主库配置此参数
         slave-sql-verify-checksum=1 #开启从服务器io_thread写入relay_log,sql_thread检验relay_log的checksum,从库配置此参数
         max_relay_log_size=1024M  #中继日志最大容量
         relay_log=/usr/local/mysql/relay_logs/relay-bin  #中继日志文件路径
         binlog-checksum=CRC32   #日志文件checksum算法
         binlog-rows-query-log_events  #开启二进制日志详细记录
         master-info-repository=TABLE  #主库信息以表格形式存储,此参数在从库配置
         relay-log-info-repository=TABLE  #中继日志信息以表格形式存储
         sync-master-info=1  #当master-info-repository=TABLE,开启则会更新master info信息,否则不更新
         sync_relay_log=1  #开启刷新relay log 事件
         replicate-ignore-db = mysql   #忽略mysql库的更新语句到从库




         #GTID复制模式
         gtid-mode=on    #开启GTID复制模式,
         enforce-gtid-consistency=true    #强制支持GTID复制模式,当个别语句不支持时无法执行


         #优化配置参数

          thread_concurrency=8   #为cpu核数的2倍
          key_buffer_size=2G     #全局缓存大小
          innodb_buffer_pool_size=8G #对innodb引擎影响很大的一个参数,根据机器内存配置
          innodb_additional_mem_pool_size=20M #数据库对象比较多时调大该参数
          innodb_log_buffer_size=20M  #不建议超过32M
          query_cache_size=512M     #查询缓存大小
          read_buffer_size=16M 
          sort_buffer_size=16M
          thread_cache_size=64   #线程缓存数大小
          join_buffer_size = 512M  #连表查询缓存大小
```

#### Mysql优化

1. 字段优化

   ```
    尽量使用明确的数据类型定义，避免使用Null类型 
   ```

2. 索引优化

   对常用的字段创建索引

3. SQL语句优化

   避免使用无法触发索引搜索的语句，如select \* , like ,join等

4. 大表拆分

5. 参数优化

   配置My.cnf,对关键参数调优,开启慢日志查询,记录慢sql语句



