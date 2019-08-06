 

1、有问题时完全卸载vsftpd
sudo apt-get purge vsftpd

2、安装vsftpd
sudo apt-get install vsftpd

3、创建用户
 

useradd -d /home/test -m test	#增加用户test，并制定test用户的主目录为/home/test
passwd test	#为test用户设置密码

sudo usermod -s /sbin/nologin test	#限定用户test不能telnet，只能ftp
sudo usermod -s /bin/bash test	#用户test恢复正常
sudo usermod -d /home/test test      #更改用户test的主目录为/test

删除用户
sudo userdel -r newuser




4、配置vsftpd.conf
sudo nano /etc/vsftpd.conf

编辑vsftpd.conf文件 如果嫌找代码麻烦，可以直接在最后添加

userlist_deny=NO
userlist_enable=YES
#允许登录的用户
userlist_file=/etc/allowed_users
seccomp_sandbox=NO
#默认ftp下载目录
local_root=/home/ftpuser/


local_enable=YES
#设置文件上传
write_enable=YES
#使用utf8
utf8_filesystem=YES


anonymous_enable=YES
anon_root=/home/……/ftp
no_anon_password=YES
write_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES



 

5、启动停止重启服务
sudo /etc/init.d/vsftpd start
sudo /etc/init.d/vsftpd stop
sudo /etc/init.d/vsftpd restart


6、访问ftp服务器
sudo ftp 127.0.0.1



浏览器访问 
ftp://127.0.0.1 
或者 
ftp://localhost

远程访问时使用实际ip ftp://your_ip

查看ip

ifconfig









### 为用户限制到指定目录


$ sudo nano /etc/vsftpd.conf


添加一下几句
# 用户登录路径，local_root 针对系统用户
local_root=/var/www/
# 锁定用户到各自目录为其根目录
chroot_local_user=YES
# anon_root 针对匿名用户
anon_root=/var/www/html
allow_writeable_chroot=YES
# 用户配置目录
user_config_dir=/etc/vsftpd/userconfig
# 不写这句上传会失败
write_enable=YES 


配置各自用户访问根目录
$ sudo mkdir /etc/vsftpd/
$ cd /etc/vsftpd/
$ sudo mkdir userconfig
$ cd userconfig/

在userconfig目录下为不同用户配置不同的根目录： 

$ sudo nano test  ：local_root=/var/www/test

$ sudo nano tony  ：local_root=/var/www/tony

#改变文件权限 把文件夹test 改为test所有 组名为nogroup,不写这句上传会失败
$ sudo chown -R test:nogroup /var/www/test


 
 
 
 
filezilla 读取目录列表失败问题

$ sudo apt-get install vsftpd

在文件末添加
pasv_min_port=2121
pasv_max_port=2142

$ sudo iptables -I INPUT -p tcp -m state --state NEW -m tcp -m multiport --dports 2121:2142 -j ACCEPT

重启 sudo /etc/init.d/vsftpd restart



