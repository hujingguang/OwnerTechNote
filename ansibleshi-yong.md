#### Ansible

> ```markdown
>  
>  自动化批量管理工具,无需安装客户端,使用Python编写,使用paramiko模块基于ssh协议和客户端通行。
>  
>  
>  官方英文文档： http://docs.ansible.com/ansible/latest/index.html
>  
>  中文文档：  http://www.ansible.com.cn/
>  
>  参考文档：  http://www.mamicode.com/info-detail-1424482.html
> ```

##### 

##### 

##### 安装

```
  pip install ansible



  Ansible默认配置文件：  /etc/ansible/ansible.cfg 


  vim  /etc/ansible/ansible.cfg

      [defaults] 
        inventory      = /etc/ansible/hosts
        host_key_checking=False
        roles_path = /etc/ansible/roles
        log_path = /tmp/ansible.log
        sudo_user=root
        remote_port=22
        host_key_checking=False
        remote_user=root
        module_name=command
        private_key_file=/root/.ssh/id_rsa
        no_log:False

  变量配置目录：   /etc/ansible/group_vars,  /etc/ansible/host_vars


  roles目录结构：

     /etc/ansible/roles/tasks

     /etc/ansible/roles/files

     /etc/ansible/roles/templates

     /etc/ansible/roles/meta

     /etc/ansible/roles/vars

     /etc/ansible/roles/handlers
```

目录结构如图

![](/assets/ansible.png)

##### 注意事项

```
   roles目录下每个模块的Yml文件必须以main 命名. 格式为yml格式(以---开头), 变量应用如果 以{ 开头要用引号引起
```

#### YML文件范例

    hello



