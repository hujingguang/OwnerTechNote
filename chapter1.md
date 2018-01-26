# OpenLDAP服务

```
    Openldap是开源的轻量目录访问协议的实现，基于x.500协议，支持TCP/IP协议，默认使用 Berkeley DB 作为后端数据库。
    Openldap里面的数据以目录结构方式存储，在查询方面优于关系型数据库，但更新操作慢。
```

1. ##### OpenLDAP安装

###### a.  Install  BDB

```
     yum install glibc-headers gcc-c++ -y    \#安装gcc编译器

     wget  http://download.oracle.com/berkeley-db/db-5.1.29.tar.gz \#获取BDB 源码包

     tar -zxvf db-5.1.29.tar.gz && cd db-5.1.29

     ../dist/configure --prefix=/usr/local/bdb5.1 --enable-compat185 --enable-dbm --enable-cxx 

     make && make  docdir=/usr/share/doc/db-5.1  install
```

###### b. Install OpenLDAP

```
     wget ftp://ftp.openldap.org/pub/OpenLDAP/openldap-release/openldap-2.4.45.tgz 

     tar -zxvf openldap-2.4.45.tgz &&  cd openldap-2.4.45

     ./configure --prefix=/usr/local/openladp  CPPFLAGS="-I/usr/local/bdb5.1/include" LDFLAGS="-L/usr/local/bdb5.1/lib -Wl,-rpath,/usr/local/bdb5.1/lib"

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

         /usr/local/openldap/sbin/ldaptest -F /usr/local/openldap/etc/openldap -f /usr/local/openldap/etc/openldap/sldap.conf   #生成 cn=config目录及动态文件，如不执行则无法启动服务！

         /usr/local/openldap/libexec/sldapd -F /usr/local/openldap/etc  #启动服务


         vim /usr/local/openldap/init.ldif   #初始化根域,保存

               dn: dc=hujingguang,dc=com
               dc: hujingguang
               objectClass: top
               objectClass: dcObject
               objectClass: organization
               o: laidian

               dn: cn=admin,dc=hujingguang,dc=com
               cn: admin
               cn: hu jing guang
               objectClass: top
               objectClass: person
               sn: jingguang


         /usr/local/openldap/bin/ldapadd -x -w your_passwod -D 'cn=amdin,dc=hujingguang,dc=com' -H ldap://127.0.0.1 -f init.ldif  #执行初始化
```

###### d. Install phpldapadmin

```
     wget  http://cn2.php.net/distributions/php-5.4.40.tar.gz  #注意版本 为5.4 ！ 

     tar -zxvf php-5.4.40.tar.gz && cd php-5.4.40 

     ./configure --prefix=/usr/local/php  --with-openssl --with-gettext --enable-fpm --with-mysql --with-mysqli --with-freetype-dir --with-jpeg-dir --with-png-dir --with-zlib --with-libxml-dir=shared --enable-xml --with-curl --with-mhash --with-mcrypt --with-gd --enable-gd-native-ttf --with-xsl --with-ldap --with-ldap-sasl --with-xmlrpc --without-pear --enable-zip --enable-soap --enable-mbstring --enable-ftp --enable-sockets --enable-pcntl --enable-bcmath --enable-shmop --enable-sysvsem --enable-inline-optimization --disable-rpath --enable-mbregex --with-xmlrpc --enable-wddx --enable-ftp --with-ldap=/usr/local/openldap

     make  && make install

     下载phpldapadmin包  地址  https://pan.baidu.com/s/1eTh1fJO 

     解压编辑 config 目录下config.php

         vim  config.php

         $servers->setValue('server','host','127.0.0.1');

         $servers->setValue('server','base',array('dc=imlaidian,dc=com'));

         $servers->setValue('login','bind_id','cn=admin,dc=imlaidian,dc=com');
```

##### 

##### 2. 命令解析

```
  ldappasswd ： 用来生成一个密码加密的字符串，在sldap.conf 中 rootpw 的值填写该字符串作为管理员的密码


  ./bin/ldapsearch -x -w 123123 -D 'cn=admin,dc=wikiki,dc=com' -H ldap://127.0.0.1 -b 'cn=hoover,dc=wikiki,dc=com'  #查询一个entry,
        -x   -- 使用简单密码访问
        -w   -- 输入的密码
        -D   -- 指定一个根标识符，表示这颗树的根
        -H   -- 访问的url
        -b   -- 要搜索的entry

  ./bin/ldapadd -x -w 123123 -D 'cn=admin,dc=wikiki,dc=com' -H ldap://127.0.0.1 -f add_user.ldif   #从一个文件创建entry
```



##### 3. 注意事项

       openldap v2.4.4 只支持 BDB v4.4 ~ v4.8 or v5.0 ~ 5.1



