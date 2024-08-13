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

# Linux常用命令

## grep搜索

| 选项  | description   |
| --- | ------------- |
| -i  | 忽略大小写         |
| -n  | 显示匹配行号        |
| -v  | 显示不包含匹配文本的所有行 |
| ^   | 指定字符开头        |
| \$  | 指定字符结尾        |
| .   | 匹配任意非换行符      |

## 压缩和解压缩

支持`gz`, `bz2`, 'zip'

## 文件权限命令

chmod

| role | Desc        |
| ---- | ----------- |
| u    | user,该文件所有者 |
| g    | group,用户组   |
| o    | other,其它用户  |
| a    | all,所有用户    |

权限设置说明

| 操作符 | Desc                        |
| --- | --------------------------- |
| +   | add authority               |
| -   | remove (revoking) authority |
| =   | 设置权限                        |

权限说明

| permission | DESC        |
| ---------- | ----------- |
| r          | read-4      |
| w          | write-2     |
| x          | execution-1 |
| -          | no          |

```bash
// no space
chmod u=rw,g=rw,o=r 1.py
```

## 获取管理员权限

```bash
sudo -s // 切换到root用户
```

退出root 

```bash
exit
whoami
```

## which 用来查询命令所在的路径

## 关机和重启

| cmd             | DESC |
| --------------- | ---- |
| shutdown -h now | 立刻关机 |
| reboot          | 重启   |

## 用户相关操作

| CMD     | DESC             |
| ------- | ---------------- |
| useradd | create new users |

命令选项

| option | DESC                         |
| ------ | ---------------------------- |
| -m     | 自动创建用户主目录，主目录的名字就是用户名        |
| -g     | 指定用户所属的用户组，默认不指定，自动创建一个同名用户组 |

查看用户信息

```bash
cat /ect/passwd
cat /etc/group
//
id hxl170008
```

给用户添加一个附加组

修改用户信息 - usermod

| CMD | DESC    |
| --- | ------- |
| -G  | 设置一个附加组 |
| -g  | 修改用户组   |

```bash
sudo usermod -G sudo laowang
```

- 删除附加值 - `gpasswd`

| Option | DESC     |
| ------ | -------- |
| -a 用户名 | 给用户添加附加组 |
| -d 用户名 | 给用户删除附加组 |

- 删除用户 `userdel`

| Option | DESC    |
| ------ | ------- |
| -r 用户名 | 删除用户主目录 |

### 创建用户组 - groupadd

### 删除用户组 - groupdel

1. 先删除用户组里的用户

2. 在删除用户组

## 文件重定向

| CMD | DESC          |
| --- | ------------- |
| >   | 如果文件存在，覆盖原有内容 |
| >>  | 保留源文件，在末尾添加   |

## cp拷贝文件

`cp -a file` 拷贝文件并保留权限，使用关键字`-a`. 也应用于文件夹之间的copy

## apt list 查看已安装的软件

## scp远程拷贝命令

## 软件安装

- 离线安装 `deb` 格式文件

- 在线安装 `apt-get`
2. deb文件格式安装

| CMD  | DESC        |
| ---- | ----------- |
| dpkg | 安装和卸载deb安装包 |

| Option | DESC       |
| ------ | ---------- |
| -i     | 离线安装deb安装包 |
| -r     | 离线卸载安装包    |

- 查看软件下载源

```bash
cat /etc/apt/source.list
```

- 在线卸载软件

```bash
sudo apt-get remove 安装包名
```
