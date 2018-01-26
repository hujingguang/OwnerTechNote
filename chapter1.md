# OpenLDAP服务

Openldap是开源的轻量目录访问协议的实现，基于x.500协议，支持TCP/IP协议，默认使用 Berkeley DB 作为后端数据库。Openldap里面的数据以目录结构方式存储，在查询方面优于关系型数据库，但更新操作慢。

1. ##### OpenLDAP安装

   ```
       a.  安装BDB

            yum install glibc-headers gcc-c++ -y    \#安装gcc编译器

            wget  http://download.oracle.com/berkeley-db/db-5.2.42.tar.gz \#获取BDB 源码包

            ../dist/configure --prefix=/usr/local/bdb5.0 --enable-compat185 --enable-dbm --enable-cxx 

            make && make  docdir=/usr/share/doc/db-5.2  install
   ```



