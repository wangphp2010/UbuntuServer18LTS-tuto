git remote add origin https://github.com/wangphp2010/UbuntuServer18LTS-tuto.git

git push -u origin master

# Ubuntu Server 网络设置

* TCP/IP 
* /etc/network/interface


  Ubuntu Server服务器ip地址设置


### 0 网址确定
```bash
$ ifconfig
```

### 1 编辑网络适配器文件
```bash
## 以前版本的ubuntu
$ cd /etc/network
$ sudo nano interfaces


...
auto lo
iface lo inet loopback

#auto eth0
#iface eth0 inet dhcp

auto eth0
iface eth0 inet static
address 192.168.11.200
netmask 255.255.255.0
network 192.168.11.0
broadcast 192.168.11.255
gateway 192.168.11.1
dns-nameservers 192.168.11.1
...

## 版本ubuntu 18+ 用下面方法

设置方法https://www.jianshu.com/p/a7b295cdd613
虚拟机中ubuntu系统的:设置-> 网络-> 网卡1-> 连接方式选择 : 桥接网卡

 
#ctrl+o 保存 ctrl+x 退出
#$ service sshd restart #重启ssh服务
$ ifconfig #查看ip地址 然后就可以用putty 或git bash 登录 $ ssh tony@192.168.56.101

```

## Ubuntu Server 系统的自动更新和手动更新

### 自动更新
**系统更新设置文件**
先查看是否有这个文件 /etc/apt/apt.conf.d/50unsttended-upgrades

如果没有则安装 $sudo apt-get install unsttended-upgrades

看一些50unsttended-upgrades 文件的内容
...
// 表示注释

"${distro_id}:${distro_codename}-security"; #表示打开了自动更新安全补丁
...


**更新文件源**
~~~
/etc/apt/sources.list
~~~


### 手动更新
```bash
# 利用更新源文件/etc/apt/sources.list 手动敲指令更新
$ sudo apt-get update

# 检查文件源版本,如果软件版本更新则提示是否更新
$ sudo apt-get upgrade

#ubuntu 核心文件的更新
$ sudo apt-get dist-upgrade

#服务器重启
$ sudo reboot
```
### 建立一个自己的系统更新文件

```bash
$ cd # 回到主目录
$ sudo nano up 
...
   sudo apt-get update
   sudo apt-get upgrade
...
# 给up文件附上可执行权限
$ sudo chmod +x up
#执行up文件
$ ./up
```

## 文本编辑器

* vi/vim
* nano 


### nano 编辑器

* CTRL + O 保存
* CTRL + X 关闭
* CTRL + K 光标所在行剪切
* CTRL + W 查询关键字词

#### 示例

```bash
 $ nano myshell
...
#!/bin/bash

# print date
date
# print System Path is $PATH
echo "System Path is $PATH"
...
$ chmod +x myshell
$ ./myshell

```

## 包管理

* 包管理文件
* 包管理命令

### 包管理文件

```bash
/etc/apt/sources.list
```

**添加Nginx源**
```bash
$ sudo nano /etc/apt/sources.list

...
deb http://nginx.org/packages/ubuntu/ trusty nginx
deb-src http://nginx.org/packages/ubuntu/ trusty nginx
...

$ sudo apt-get update
$ sudo apt-get upgrade


```

问题 公钥的缺失 NO_PUBKEY 
```bash
# sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys [缺失公钥的数字签名]
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 8D5A09DC9B929006

```

### 包管理命令
* apt-get
    + 软件安装,删除,它帮助我们解决了软件包之间的依赖问题,类似redhat系统的rpm
* apt-cache
    + 软件包信息查询工具,比如,软件查找,信息确认等.
* tasksel
    + 软件包安装任务,可以让我们一次安装一套软件包,比如lamp服务器 (apache+mysql+php)
* dpkg
    + 与apt-get类似的包管理工具,但更加底层,可以执行很对apt-get不能执行的功能

**apt-get常用命令选项**

用法：apt-get [选项] 命令  
 apt-get [选项] install|remove pkg1 [pkg2 ...]  
 apt-get [选项] source pkg1 [pkg2 ...]  
  
apt-get 是一个下载安装软件包的简单命令行接口。  
最常用的命令是update(更新)  
和install(安装)。  
  
|命令：  |
|:--------|:-------- |
| 命令选项 | 功能描述 |
| update           |    软件包列表更新   |
| upgrage          | 软件包安装更新      |
| install          | 安装软件包          |
| remove           | 卸载软件包          |  
| autoremove       | 自动卸载不再需要的下载包          |  
| purge            |  永久删除一个软件包,包括设置文件等       |  
| source           |  下载源码包          |  
| build-dep        | 为某个软件包设置编译依赖          |  
| dist-upgrade     | 系统核心更新或某个包更新          |  
| clean            | 删除本地缓存          |  
| autoclean        |      删除本地不再使用的缓存    |  
| check            | 检查软件包依赖关系          |  
| changelo         | 下载显示软件包的更新履历          |  
| download         | 下载指定软件包到当前目录          |  


| 选项：  | 
|:--------|:-------- |
| -h | 本帮助文件。  | 
|  -q | 输出到日志 - 无进展指示 |  
|  -qq | 不输出信息，错误除外 |  
|  -d | 仅下载 - 不安装或解压归档文件  | 
|  -s | 不实际安装。模拟执行命令  | 
|  -y | 假定对所有的询问选是，不提示  | 
|  -f | 尝试修正系统依赖损坏处  | 
|  -m | 如果归档无法定位，尝试继续  | 
|  -u | 同时显示更新软件包的列表 |  
|  -b | 获取源码包后编译  | 
|  -V | 显示详细的版本号 |  
|  -c=? | 阅读此配置文件  | 
|  -o=? | 设置自定的配置选项，如 -o dir::cache=/tmp| 

### 安装一个网络扫描工具nmap

```bash
$ cd 
$ apt-cache show nmap # 显示该软件包具体内容
$ sudo apt-get install nmap
$ nmap www.google.com

$ sudo apt-get purge nmap # 卸载该软件包
$ nmap www.google.com

```


## 安装apache2

### 1 安装
```bash
# 安装之前确认一下安装包信息
$ apt-cache show apache2

#确认本地tcp端口使用情况
$ nmap 127.0.0.1

$ sudo apt-get install apache2
$ nmap 127.0.0.1

#测试刚刚安装的服务器
$ curl http://127.0.0.1
```
### 2 配置

**目录: /etc/apache2**
* apache2.conf
    + 全局配置文件,记载服务器基本设置内容,一半轻易不需要改变
* conf-available和conf-enabled(a2enconf,a2disconf)
    + 服务器配置文件,主要用于apache内部参数设置,例如:字符集,安全配置等
* mods-available 和mods-enabled(a2enmod,a2dismod)
    + 服务器的模块配置,也是apache扩展功能配置,
* sites-available和sites-enabled(a2ensite,a2dissite)
    + 内部站点的配置,可以用一台apache服务器配置多个站点,同时也可以在这里配置ssl网站
* envvars
    + 存放环境变量
* magic 
    + http传输的多媒体类型设置,一半不需要修改
* prots.conf
    + 服务器端口配置,一半http为80,https为443

### 3 ssl模块

```bash
#查看一安装模块
$ cd  mods-available
$ ls
#启用模块
$ sudo a2enmod ssl
#禁用模块
$ sudo a2dismod ssl
#重启apach
$ sudo service apache2 restart / systemctl restart apache2

```

### 4 设置服务器名称

```bash
$ sudo nano /etc/apache2/apache2.conf
...
ServerName myvideo.com
...
# 重启apache
$ sudo service apache2 restart / systemctl restart apache2

```
### 5 apache 文档目录的修改

```bash
$ cd /etc/apache2
$ ll sites-available/ #查看哪些网站可用
$ ll sites-enabled/ # 查看使用中的网站
$ sudo a2dissite 000-default # 禁用该网站
$ systemctl reload apache2 # 重新载入
$ curl http://127.0.0.1
$ sudo nano sites-available/myvideo.conf #配置自己的网站

...
<VirtualHost *:80>
    ServerName www.myvideo.com
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/myvideo
    ErrorLog ${APACHE_LOG_DIR}/myvideo_error.log
    CustomLog ${APACHE_LOG_DIR}/myvideo_acces.log combined
</VirtualHost>
...

$ sudo mkdir -p /var/www/myvideo
$ sudo chown tony:1234 -R /var/www/myvideo/ # 一般权限
$ cd /var/www/myvideo
$ nano index.html
...html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
   
    <title>myvideo</title>
</head>
<body>
    <h1> MYVIDEO home site</h1>
    
</body>
</html>
...
#启动刚刚设置的网站
$ sudo a2ensite myvideo
# 重启服务器
$ sudo service apache2 restart

```

