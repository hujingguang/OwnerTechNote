##### Openvpn

```
一款开源的vpn协议实现工具，通过openssl工具对传输数据加密。使用udp或tcp协议进行数据传输。

  openvpn 有两种模式： 
             server模式：  支持多个客户端，且客户端之间能过通信
             client to client 模式： 只能客户端和服务器端通信

  TUN设备： 用来通过IP协议进行互联网的通信  Tap设备： 允许局域网内的以太网帧的非IP协议通信

  openvpn使用证书双向认证，客户端和服务器将各自的证书向CA根进行认证，认证通过则建立链接，进行通信

  openssl有关名词解析：

       server.crq:    证书请求文件，用来向ca请求生成证书使用，包含了公钥等信息
       server.crt:    服务器证书文件
       server.key:    服务器私钥文件
       dh.pem:        迪菲-赫尔曼密钥交换所需要用到的字符串

       证书请求文件和私钥由openssl工具使用CA的公司公私密钥同时生成
```

###### 

###### Install

```
     root@host ]#    yum install openvpn -y
```

###### 

###### Configure

```
        wget https://github.com/OpenVPN/easy-rsa/archive/2.2.2.tar.gz   #下载公私密钥生成工具
        tar -zxvf 2.2.2.tar.gz && cd  ./easy-rsa-2.2.2/easy-rsa/2.0
        source vars && ./clear-all 
        ./build-dh         # 生成dh 密钥交换字符串
        ./build-ca         #生成根证书文件 ，Common Name项需填写,其他直接回车
        ./pkitool --server server  #生成服务器证书文件。
        ./pkitool client         #生成客户端证书文件
        iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
        echo 'net.ipv4.ip_forward=1' >> /etc/sysctl.conf && sysctl -p


        vim   /etc/openvpn/openvpn.conf  #服务器端配置

              local 192.168.10.238
              port 1194
              proto udp
              dev tun
              ca /etc/openvpn/ca.crt
              cert /etc/openvpn/server1.crt
              /etc/openvpn/server1.key  # This file should be kept secret
              dh /etc/openvpn/dh2048.pem
              server 10.8.0.0 255.255.255.0
              ifconfig-pool-persist /etc/openvpn/ipp.txt
              push "redirect-gateway def1 bypass-dhcp"
              push "dhcp-option DNS 114.114.114.114"
              keepalive 10 120
              cipher AES-256-CBC
              persist-key
              persist-tun
              status /tmp/openvpn-status.log
              log         /tmp/openvpn.log
              verb 3
              explicit-exit-notify 1

        vim   client.ovpn     #客户端配置

              client
              dev tun
              proto udp
              remote 192.168.10.238 1194 
              resolv-retry infinite
              nobind
              persist-key
              persist-tun
              ca ca.crt 
              cert client1.crt
              key client1.key
              comp-lzo
              verb 3


   修改Openvpn使用密码进行认证,并调用外部脚本读取用户名密码返回值认证，编辑openvpn.conf文件 

     auth-user-pass-verrify  /root/check_passwd.py via-env   #通过脚本验证用户名密码，返回码为0则成功，否则失败，via-env的意思为通过环境变量将用户名密码传递给脚本
     client-cert-not-required    #表示客户端不使用密钥验证，去掉则需要密码和密钥！
     username-as-common-name    #登陆用户名作为普通用户名传递
     script-security 3 execve  # 脚本安全级别，数字越低则表示越严格，3表示可以调用用户的脚本进行验证，并且可以传递用户名参数，execve表示通过调用脚本传递 2.3以上默认都用execve,可以不显式配置


  修改客户端配置
      ;cert client1.crt    
      ;key  client1.key  
      auth-user-pass
```

###### 

###### 注意问题

```
   可以连接到vpn 服务器，但是无法连接外网 ，解决方案：
   
   
     a.  打开系统转发功能  echo 1 > /proc/sys/net/ipv4/ip_forward

     b.  添加Iptales nat。 iptables -t nat -A POSTROUTING -o eth0 -j SNAT -s 10.8.0.0/24 --to-source your_public_ip_address  OR  iptables -t nat -A POSTROUTING -o eth0 -s 10.8.0.0/24 -j MASQUERADE 


   参考文档：  
     
      https://www.jianshu.com/p/4bbf946222d5
```



