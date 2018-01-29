### LNMP

LNMP \(Linux , Nginx,PHP,Mysql\),互联网web服务后台通用基础架构的简称,以下是基于centos6.5,php5.6,nginx1.12.2,Mysql5.6

的安装.

1. Nginx安装

   ```
   wget http://nginx.org/download/nginx-1.12.2.tar.gz  #获取源码包

   tar -zxvf nginx-1.12.2.tar.gz && cd nginx-1.12.2

   yum install gcc openssl openssl-devel pcre pcre-devel -y  && useradd -M -s /sbin/nologin www

   ./configure --prefix=/usr/local/nginx --user=www --group=www --with-file-aio --with-http_ssl_module --with-http_v2_module --with-http_realip_module  --with-http_stub_status_module    --with-pcre
   ```



