 

1��������ʱ��ȫж��vsftpd
sudo apt-get purge vsftpd

2����װvsftpd
sudo apt-get install vsftpd

3�������û�
 

useradd -d /home/test -m test	#�����û�test�����ƶ�test�û�����Ŀ¼Ϊ/home/test
passwd test	#Ϊtest�û���������

sudo usermod -s /sbin/nologin test	#�޶��û�test����telnet��ֻ��ftp
sudo usermod -s /bin/bash test	#�û�test�ָ�����
sudo usermod -d /home/test test      #�����û�test����Ŀ¼Ϊ/test

ɾ���û�
sudo userdel -r newuser




4������vsftpd.conf
sudo nano /etc/vsftpd.conf

�༭vsftpd.conf�ļ� ������Ҵ����鷳������ֱ����������

userlist_deny=NO
userlist_enable=YES
#�����¼���û�
userlist_file=/etc/allowed_users
seccomp_sandbox=NO
#Ĭ��ftp����Ŀ¼
local_root=/home/ftpuser/


local_enable=YES
#�����ļ��ϴ�
write_enable=YES
#ʹ��utf8
utf8_filesystem=YES


anonymous_enable=YES
anon_root=/home/����/ftp
no_anon_password=YES
write_enable=YES
anon_upload_enable=YES
anon_mkdir_write_enable=YES



 

5������ֹͣ��������
sudo /etc/init.d/vsftpd start
sudo /etc/init.d/vsftpd stop
sudo /etc/init.d/vsftpd restart


6������ftp������
sudo ftp 127.0.0.1



��������� 
ftp://127.0.0.1 
���� 
ftp://localhost

Զ�̷���ʱʹ��ʵ��ip ftp://your_ip

�鿴ip

ifconfig









### Ϊ�û����Ƶ�ָ��Ŀ¼


$ sudo nano /etc/vsftpd.conf


���һ�¼���
# �û���¼·����local_root ���ϵͳ�û�
local_root=/var/www/
# �����û�������Ŀ¼Ϊ���Ŀ¼
chroot_local_user=YES
# anon_root ��������û�
anon_root=/var/www/html
allow_writeable_chroot=YES
# �û�����Ŀ¼
user_config_dir=/etc/vsftpd/userconfig
# ��д����ϴ���ʧ��
write_enable=YES 


���ø����û����ʸ�Ŀ¼
$ sudo mkdir /etc/vsftpd/
$ cd /etc/vsftpd/
$ sudo mkdir userconfig
$ cd userconfig/

��userconfigĿ¼��Ϊ��ͬ�û����ò�ͬ�ĸ�Ŀ¼�� 

$ sudo nano test  ��local_root=/var/www/test

$ sudo nano tony  ��local_root=/var/www/tony

#�ı��ļ�Ȩ�� ���ļ���test ��Ϊtest���� ����Ϊnogroup,��д����ϴ���ʧ��
$ sudo chown -R test:nogroup /var/www/test


 
 
 
 
filezilla ��ȡĿ¼�б�ʧ������

$ sudo apt-get install vsftpd

���ļ�ĩ���
pasv_min_port=2121
pasv_max_port=2142

$ sudo iptables -I INPUT -p tcp -m state --state NEW -m tcp -m multiport --dports 2121:2142 -j ACCEPT

���� sudo /etc/init.d/vsftpd restart



