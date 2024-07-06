# 公网地址和局域网地址

好比寄快递：详细的地址

简要的地址

## 局域网IP地址

用于一个群体内的数据交换的IP地址，离开这个区域改地址就失效。



## 公网IP地址

改地址在整个网络唯一存在

- 公网IP: 123.206.16.61      123.206.16.66

- 局域网：192.168.178.123        192.168.178.144

# What is DNS

访问网站域名，首先进行`域名解析`

## 域名和IP的关系

DNS 一个电话簿

nslookup命令产看域名解析关系



# Linux的DNS配置文件

1. /etc/hosts文件，运维人员自由定义域名和IP的关系
   
   - vim /etc/hosts 写入记录 `127.0.0.1 www.pythonav.cn`
   
   - ping www.pythonav.cn 解析到127.0.0.1.因此hosts文件的解析优先级高于DNS

2. /etc/resolv.cof, 该文件填入互联网DNS服务器的地址
   
   - it records the address of DNS server

## DNS服务端的配置和DNS劫持

- /etc/hosts文件：自己本地的电话簿

- /etc/resolv.conf：dns服务器地址的配置文件，dns服务器是专门搜集世界所有的域名

- 
