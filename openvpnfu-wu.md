##### Openvpn

```
一款开源的vpn协议实现工具，通过openssl工具对传输数据加密。使用udp或tcp协议进行数据传输。

  openvpn 有两种模式： 

             server模式：  支持多个客户端，且客户端之间能过通信

             client to client 模式： 只能客户端和服务器端通信
```

###### Install

```
     root@host \]\#    yum install openvpn -y
```

###### 

###### Configure

```
        wget https://github.com/OpenVPN/easy-rsa/archive/2.2.2.tar.gz   #下载公私密钥生成工具
```



