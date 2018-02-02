### LNMP

LNMP \(Linux , Nginx,PHP,Mysql\),互联网web服务后台通用基础架构的简称,以下是基于centos6.5,php5.6,nginx1.12.2,Mysql5.6

的安装.

1. **Nginx安装**

   ```
   wget http://nginx.org/download/nginx-1.12.2.tar.gz  #获取源码包
   tar -zxvf nginx-1.12.2.tar.gz && cd nginx-1.12.2
   yum install gcc openssl openssl-devel pcre pcre-devel -y  && useradd -M -s /sbin/nologin www
   ./configure --prefix=/usr/local/nginx --user=www --group=www --with-file-aio --with-http_ssl_module --with-http_v2_module --with-http_realip_module  --with-http_stub_status_module    --with-pcre
   make && make install
   chown -R www.www /usr/local/nginx
   /usr/local/nginx/sbin/nginx   # 启动Nginx服务
   ```

2. **Mysql安装**  
           yum install ncurses-devel zlib-devel perl-DBI perl-DBD-mysql perl-Time-HiRes perl-IO-Socket-SSL perl-Term-ReadKey cmake -y   
          wget[http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.6/mysql-5.6.38.tar.gztar](http://ftp.ntu.edu.tw/MySQL/Downloads/MySQL-5.6/mysql-5.6.38.tar.gztar) -zxvf mysql-5.6.38.tar.gz && cd mysql-5.6.38  
          useradd -M -s /sbin/nologin mysql   
          cmake -DCMAKE\_INSTALL\_PREFIX=/usr/local/mysql -DSYSCONFDIR=/usr/local/mysql/my.cnf -DMYSQL\_DATADIR=/usr/local/mysql/data -DWITH\_INNOBASE\_STORAGE\_ENGINE=1 -DWITH\_MEMORY\_STORAGE\_ENGINE=1 -DWITH\_MYISAM\_STORAGE\_ENGINE=1 -DWITH\_ARCHIVE\_STORAGE\_ENGINE=1 -DWITH\_READLINE=1 -DENABLED\_LOCAL\_INFILE=1 -DDEFAULT\_CHARSET=utf8 -DDEFAULT\_COLLATION=utf8\_general\_ci -DEXTRA\_CHARSET=utf8 -DWITH\_USER=mysql -DWITH\_EMBEDDED\_SERVER=OFF  
          make && make install  
          vim /usr/local/mysql/my.cnf  \#配置mysql配置文件  
          innodb\_buffer\_pool\_size = 128M  
   basedir = /usr/local/mysql  
   datadir = /usr/local/mysql/data  
   port = 3306  
   server\_id = 1  
   pid-file=/usr/local/mysql/var/mysql.pid  
   user=mysql

bind-address=192.168.10.238

skip-name-resolve  
back\_log=1200  
max\_connections=2000  
transaction\_isolation = REPEATABLE-READ  
log\_bin=/usr/local/mysql/logs/mysql-bin  
binlog\_format=row  
max\_binlog\_size = 2G  
gtid-mode=on  
enforce-gtid-consistency=true  
binlog-rows-query-log\_events=1  
log-slave-updates=1  
skip\_slave\_start=1  
expire\_logs\_days=7  
log\_error=/usr/local/mysql/var/error.log  
slow\_query\_log=1  
long\_query\_time=1  
slow\_query\_log\_file=/usr/local/mysql/logs/slow.log  
innodb\_file\_per\_table = 1  
default-storage-engine=InnoDB  
join\_buffer\_size = 128M  
sort\_buffer\_size = 2M  
read\_rnd\_buffer\_size = 2M  
sql\_mode=NO\_ENGINE\_SUBSTITUTION,STRICT\_TRANS\_TABLES

cp /usr/local/mysql/support-files/mysql.server /etc/init.d/mysqld   
   sed -i 's\#database=\#database=/usr/local/mysql/data\#g' /etc/init.d/mysqld  
   sed -i 's\#basedir=\#basedir=/usr/local/mysql\#' /etc/init.d/mysqld  
   chmod +x /etc/init.d/mysqld   
   mkdir /usr/local/mysql/{var,logs}  
   chown -R mysql.mysql /usr/local/mysql  
   cd /usr/local/mysql && ./scripts/mysql\_install\_db --user=mysql --defaults-file=/usr/local/mysql/my.cnf --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data

\`\`\`

```
 Mysql Cmake options 说明文档：[https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.htm](https://dev.mysql.com/doc/refman/5.6/en/source-configuration-options.html)
```

##### 3. **Php安装**



