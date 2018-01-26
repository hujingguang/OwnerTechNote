### **Samba 搭建**

##### samba安装

yum install samba-common samba  samba-client tdb-tools -y

##### samba配置

\`\`\`  
 vim  /etc/samba/smb.conf

```bash
    load printers = no
    printing = bsd
    printcap name = /dev/null
    disable spoolss = yes 
    display charset = UTF8
    dos charset = cp950
    unix charset = UTF8
    workgroup = WORKGROUP
    server string = Samba Server Version %v
    netbios name = SMB Server
    hosts allow = 127. 192.168.10. 
    include = /etc/samba/%U.smb.conf
    log file = /var/log/samba/samba.log
    max log size = 50
    security = user
    passdb backend = tdbsam

    [public]
      comment = 公共资源
      path = /data/samba/public
      browseable = yes
      public = yes
      writable = yes
```

![](/assets/samba1.png)

vim  /etc/samba/smb.d/%U.smb.conf

```
        [软件技术部]
         comment = 软件技术部内部目录
         path    = /data/samba/软件技术部
         public  = yes
         browseable = yes
         read list = @dev_group
         valid users = @dev_group
         writelist = @dev_group
```

##### 创建用户

```bash
       root@smb ]#  groupadd ops_group
       root@smb ]#  useradd -M -s /sbin/nologin -g ops_group ops
       root@smb ]#  pdbedit -a -u ops   #添加samba用户,需在linux系统用户中存在!
```

##### 创建共享目录

```
        root@smb ]#  mkdir /data/samba/public    
        root@smb ]#  mkdir /data/samba/软件技术部  
        root@smb ]#  chown -R ops.ops_group /data/samba/软件技术部
        root@smb ]#  chown -R ops.ops_group /data/samba/软件技术部
        root@smb ]#  chmod -R 775 /data/samba/软件技术部  
        root@smb ]#  chmod -R 775 /data/samba/public
```

##### 配置服务

```
              root@smb ]#  chkconfig --level 345 smb on
              root@smb ]#  chkconfig --level 345 nmb on
```

### Samba目录配置说明

1. 目录结构

   Public目录为公共可读目录，下面有各部门对外开放的子目录，其他人可下载阅读，但是只有相关部门可以在其对应的目录下上传文件。另置一个公共读写目录,所有账号都可以上传下载里面的文件,但不能删除其他人上传的文件。

   Private目录为各部门内部目录，只对部门内部账号可见，其他账号无法查看，与public目录平级,以部门名命名。如下图

   ![](/assets/samba2.jpg)

### Samba访问

1. Window访问地址

   ```
     打开我的电脑  地址栏输入  \\192.168.10.199   输入用户名和密码
   ```

2. 切换账号

   ```
   先将打开的共享窗口关闭， 运行cmd     输入命令:     net use \\192.168.10.199 /del /y   然后等待 30s  不要立即再访问，否则登陆信息缓存仍然存在!  (若无效果,注销window用户即可使登陆信息失效)
   ```

3. 密码修改

   ```
    打开浏览器   http://192.168.10.199?username=hr&old_passwd=123123&new_passwd=456456

     username=hr       中的 hr 为用户名  
     old_passwd=123123 中的123123为旧密码  
     new_passwd=456456 中的456456为新密码
   ```



Github Address: https://github.com/hujingguang/tech\_note

