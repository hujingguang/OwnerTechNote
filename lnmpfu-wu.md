### LNMP

LNMP \(Linux , Nginx,PHP,Mysql\),互联网web服务后台通用基础架构的简称,以下是基于centos6.5,php5.6,nginx1.12.2,Mysql5.6

的安装.

1. Nginx安装

   ```
   wget http://nginx.org/download/nginx-1.12.2.tar.gz  #获取源码包
   tar -zxvf nginx-1.12.2.tar.gz && cd nginx-1.12.2
   yum install gcc openssl openssl-devel pcre pcre-devel -y  && useradd -M -s /sbin/nologin www
   ./configure --prefix=/usr/local/nginx --user=www --group=www --with-file-aio --with-http_ssl_module --with-http_v2_module --with-http_realip_module  --with-http_stub_status_module    --with-pcre
   make && make install
   chown -R www.www /usr/local/nginx
   /usr/local/nginx/sbin/nginx   # 启动Nginx服务
   ```

2. Mysql安装

   ```
        yum install ncurses-devel zlib-devel perl-DBI perl-DBD-mysql perl-Time-HiRes perl-IO-Socket-SSL perl-Term-ReadKey cmake -y
        wget http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.6/mysql-5.6.38.tar.gz
        tar -zxvf mysql-5.6.38.tar.gz  && cd mysql-5.6.38 
        useradd -M -s /sbin/nologin mysql
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DSYSCONFDIR=/usr/local/mysql/my.cnf -DMYSQL_DATADIR=/usr/local/mysql/data -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DENABLED_LOCAL_INFILE=1 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DEXTRA_CHARSET=utf8 -DWITH_USER=mysql -DWITH_EMBEDDED_SERVER=OFF
        make && make install
        vim /usr/local/mysql/my.cnf   #配置mysql配置文件

             innodb_buffer_pool_size = 128M
             basedir = /usr/local/mysql
             datadir = /usr/local/mysql/data
             port = 3306
             server_id = 1
             pid-file=/usr/local/mysql/var/mysql.pid
             user=mysql
             #bind-address=192.168.10.238
             skip-name-resolve
             back_log=1200
             max_connections=2000
             transaction_isolation = REPEATABLE-READ
             log_bin=/usr/local/mysql/logs/mysql-bin
             binlog_format=row
             max_binlog_size = 2G
             gtid-mode=on
             enforce-gtid-consistency=true 
             binlog-rows-query-log_events=1
             log-slave-updates=1  
             skip_slave_start=1  
             expire_logs_days=7
             log_error=/usr/local/mysql/var/error.log
             slow_query_log=1
             long_query_time=1
             slow_query_log_file=/usr/local/mysql/logs/slow.log
             innodb_file_per_table = 1
             default-storage-engine=InnoDB
             join_buffer_size = 128M
             sort_buffer_size = 2M
             read_rnd_buffer_size = 2M 
             sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
   ```

   Mysql Cmake options 说明文档：[https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html](https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html)



