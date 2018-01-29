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
        cmake -DCMAKE_INSTALL_PREFIX=/usr/local/mysql -DMYSQL_DATADIR=/usr/local/mysql/data -DWITH_INNOBASE_STORAGE_ENGINE=1 -DWITH_MEMORY_STORAGE_ENGINE=1 -DWITH_MYISAM_STORAGE_ENGINE=1 -DWITH_ARCHIVE_STORAGE_ENGINE=1 -DWITH_READLINE=1 -DENABLED_LOCAL_INFILE=1 -DDEFAULT_CHARSET=utf8 -DDEFAULT_COLLATION=utf8_general_ci -DEXTRA_CHARSET=utf8 -DWITH_USER=mysql -DWITH_EMBEDDED_SERVER=OFF 
   ```



