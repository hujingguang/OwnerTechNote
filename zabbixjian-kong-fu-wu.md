#### **Zabbix**

zabbix是一款企业级分布式监控软件,集成图表展示,报警,分布式监控.支持自定义监控脚本,能支持机器数量5000台范围内的一款

监控解决方案。以下是基于zabbix3.4部署文档。

##### requirements

centos6.x , mysql5.6 , zabbix3.4, php5.6

安装PHP5.6  Mysql Server Nginx 请参照 [LNMP服务](/lnmpfu-wu.md) 章节

#### zabbix安装

```
 wget  https://cfhcable.dl.sourceforge.net/project/zabbix/ZABBIX%20Latest%20Stable/3.4.7/zabbix-3.4.7.tar.gz

 tar -zxvf zabbix-3.4.7.tar.gz && cd zabbix-3.4.7

 yum install net-snmp-devel OpenIPMI OpenIPMI-devel libevent libevent-devel mysql mysql-devel -y 

 ./configure  --prefix=/usr/local/zabbix --enable-server  --enable-agent  --with-mysql=/usr/local/mysql/bin/mysql_config --with-libxml2  --with-net-snmp --with-openipmi  --with-openssl --enable-ipv6  --with-libcurl

 ln -s /usr/local/mysql/lib/libmysqlclient.so.18 /usr/lib64/

 useradd -M -s /sbin/nologin zabbix

 cd /root/zabbix-3.4.7 && cp misc/init.d/fedora/core/zabbix_agentd /etc/init.d/ && cp misc/init.d/fedora/core/zabbix_server /etc/init.d/

 sed -i 's#BASEDIR=/usr/local# BASEDIR=/usr/local/zabbix#/g' /etc/init.d/zabbix_agentd 

 sed -i 's#BASEDIR=/usr/local# BASEDIR=/usr/local/zabbix#/g' /etc/init.d/zabbix_server
 
 cd /root/zabbix-3.4.7/database/mysql
 
 mysql -u zabbix -pzabbix -h 192.168.10.199 --default-character-set=utf8  zabbix <./schema.sql
 mysql -u zabbix -pzabbix -h 192.168.10.199 --default-character-set=utf8  zabbix <./images.sql
 mysql -u zabbix -pzabbix -h 192.168.10.199 --default-character-set=utf8  zabbix <./data.sql




 vim /usr/local/php/lib/php.ini

    post_max_size = 16M
    always_populate_raw_post_data = -1
    default_socket_timeout = 60
    date.timezone = "Asia/Shanghai"

 mkdir /data/www/zabbix  && cd /root/zabbix-3.4.7/frontends/php && cp -a * /data/www/zabbix/ && chown -R www.www /data/www/zabbix

 vim /data/www/zabbix/conf/zabbix.conf.php  #配置数据库

$DB['TYPE']                             = 'MYSQL';
$DB['SERVER']                   = '192.168.10.199';
$DB['PORT']                             = '3306';
$DB['DATABASE']                 = 'zabbix';
$DB['USER']                             = 'zabbix';
$DB['PASSWORD']                 = 'zabbix';   


   vim  /usr/local/nginx/conf/nginx.conf

          server {
        listen       80;
        server_name  192.168.10.199;
        location / {
            root   /data/www/zabbix;
            index  index.html index.htm index.php;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }


       location ~ \.php$ {
           root           /data/www/zabbix;
           fastcgi_pass   127.0.0.1:9000;
           fastcgi_index  index.php;
           fastcgi_param  SCRIPT_FILENAME  /data/www/zabbix$fastcgi_script_name;
           include        fastcgi_params;
       }
    }      

 /etc/init.d/mysqld start
 cd /usr/local/php && ./sbin/php-fpm
 /etc/init.d/zabbix_server start
 /etc/init.d/zabbix_agentd start
 /usr/local/nginx/sbin/nginx

 web 浏览器打开 192.168.10.199    #默认用户名密码  admin/zabbix
```



