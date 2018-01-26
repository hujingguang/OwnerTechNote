# OpenLDAP服务

Openldap是开源的轻量目录访问协议的实现，基于x.500协议，支持TCP/IP协议，默认使用 Berkeley DB 作为后端数据库。Openldap里面的数据以目录结构方式存储，在查询方面优于关系型数据库，但更新操作慢。

1. ##### OpenLDAP安装

###### a.  Install  BDB

```
     yum install glibc-headers gcc-c++ -y    \#安装gcc编译器

     wget  http://download.oracle.com/berkeley-db/db-5.2.42.tar.gz \#获取BDB 源码包

     tar -zxvf db-5.2.42.tar.gz && cd db-5.2.42

     ../dist/configure --prefix=/usr/local/bdb5.2 --enable-compat185 --enable-dbm --enable-cxx 

     make && make  docdir=/usr/share/doc/db-5.2  install
```

###### b. Install OpenLDAP

```
     wget ftp://ftp.openldap.org/pub/OpenLDAP/openldap-release/openldap-2.4.45.tgz 

     tar -zxvf openldap-2.4.45.tgz &&  cd openldap-2.4.45

     ./configure --prefix=/usr/local/openladp  CPPFLAGS="-I/usr/local/bdb5.2/include" LDFLAGS="-L/usr/local/bdb5.2/lib -Wl,-rpath,/usr/local/bdb5.2/lib"

     make depend && make && make install
```

###### c. Configure OpenLDAP

```
         /usr/local/openldap/sbin/ldappasswd   \# 执行会生成密码 

         vim  /usr/local/openldap/etc/sldap.conf

               database      mdb
               maxsize       1073741824
               suffix        "dc=hujingguang,dc=com"
               rootdn        "cn=admin,dc=hujingguang,dc=com"        #此处修改成自己的DN
               rootpw        {SSHA}BT48KryUWpPyIDIw8KEDgxsVCuVL4WbF  # 此处填写生成的密码
```



