##### Openvpn

```
一款开源的vpn协议实现工具，通过openssl工具对传输数据加密。使用udp或tcp协议进行数据传输。

  openvpn 有两种模式： 

             server模式：  支持多个客户端，且客户端之间能过通信

             client to client 模式： 只能客户端和服务器端通信
             
  TUN设备： 用来通过IP协议进行互联网的通信  Tap设备： 允许局域网内的以太网帧的非IP协议通信
```

###### Install

```
     root@host \]\#    yum install openvpn -y
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
```



