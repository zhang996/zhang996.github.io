---
title: Debian部署Samba共享文件
date: 2020-10-11 12:00:00
tags:
---

​        Samba是在Linux和UNIX系统上实现SMB协议的一个免费软件，由服务器及客户端程序构成。Linux下的Samba服务主要用于Windows平台和linux平台下载局域网内实现文件共享。只包含一个配置文件，方便部署。

**一、安装samba**

​        在root权限下执行`apt-get install samba`

**二、创建共享目录**

```
	mkdir /usr/download                  //创建要共享的目录

	chmod 777 /usr/download              //修改权限
```

**三、 配置文件**

​         配置文件在/etc/samba/smb.conf

​        有三个特殊块[global]、[homes]、[printers]具体有以下内容:

```
	[global]:此块中的参数作用于整个server，或作为其他块中没有定义的一些参数的默认值。参见参数PARAMETERS以获取更多信息。
	[homes]:如果配置文件中有[homes]块，则会把此用户的home路径共享到此处，既可以访问当前samba用户对应的home文件夹。
	[printer]:这个用于共享打印服务，很少用到。
```

**注意：每次修改了配置文件之后先运行`testparm`来检测语法**

​           **修改完后执行 `service smdb restart` 或 `systemctl restart smbd.service`重启服务使其生效**

```
======================= Global Settings =======================
[global]
   workgroup = WORKGROUP
   log file = /var/log/samba/log.%m
   max log size = 1000
   logging = file
   panic action = /usr/share/samba/panic-action %d
   server role = standalone server
   obey pam restrictions = yes
   unix password sync = yes
   passwd program = /usr/bin/passwd %u
   passwd chat = *Enter\snew\s*\spassword:* %n\n *Retype\snew\s*\spassword:* %n\n *password\supdated\ssuccessfully* .
   pam password change = yes
   map to guest = bad user
   usershare allow guests = yes

======================= Share Definitions =======================
[homes]
   comment = Home Directories
   browseable = no
   read only = yes
   create mask = 0700
   directory mask = 0700
   valid users = %S
[anonymous]
   path = /usr/download 
   force group = users
   create mask = 0660
   directory mask = 0771
   browsable =yes
   writable = yes
   guest ok = yes
```

---------------------------------

**解析：**

```
[global]
config file = /etc/samba/smb.conf.%U          #可以让你使用另一个配置文件来覆盖缺省的配置文件。如                                                 果文件 不存在，则该项无效。
workgroup = WORKGROUP               　　       #工作组名称
server string = Samba Server Version %v  　 　 #主机的简易说明
netbios name = MYSERVER         	          #主机的netBIOS名称，如果不填写则默认服务器DNS的一部                                               分，workgroup和netbios name名字不要设置成一样
interfaces = lo eth0 192.168.12.2/24 192.168.13.2/24     #设置samba服务器监听网卡，可以写网卡名                                                          称或IP地址，默认注释
hosts allow = 127. 192.168.12. 192.168.13.               #设置允许连接到samba服务器的客户端，默                                                          认注释
hosts deny =192.168.12.0/255.255.255.0       #设置不允许连接到samba服务器的客户端，默认注释
log level =1                                 #日志文件安全级别，0~10级别，默认0
log file = /var/log/samba/%m  　　　　　		  #产生日志文件的命名，默认以访问者IP地址命名
max log size = 50   　　　　　　　				#日志文件最大容量50，默认50，单位为KB，0表示不限制
security = share   　　　　　　　　　　　　　　　#设置用户访问samba服务器的验证方式 ，一共四种验证方式。
	1. share：用户访问Samba Server不需要提供用户名和口令, 安全性能较低。
	2. user：Samba Server共享目录只能被授权的用户访问,由Samba Server负责检查账号和密码的正确性。账号和密码要在本Samba Server中建立。
	3. server：依靠其他Windows NT/2000或Samba Server来验证用户的账号和密码,是一种代理验证。此种安全模式下,系统管理员可以把所有的Windows用户和口令集中到一个NT系统上,使用Windows NT进行Samba认证, 远程服务器可以自动认证全部用户和口令,如果认证失败,Samba将使用用户级安全模式作为替代的方式。
	4. domain：域安全级别,使用主域控制器(PDC)来完成认证。

passdb backend = tdbsam   　　　　　　　　　　　　#定义用户后台类型
	1、smbpasswd:使用SMB服务的smbpasswd命令给系统用户设置SMB密码
	2、tdbsam:创建数据库文件并使用pdbedit建立SMB独立用户，smbpasswd –a username建立samba用户并设置密码，不过建立samba用户必须先建立系统用户
	使用以下命令添加新用户
    useradd tom -m -G users
	再使用
    smbpasswd -a tom
	3、ldapsam:基于LDAP服务进行账户验证
	username map = /etc/samba/smbusers   #配合/etc/samba/smbusers文件设置虚拟用户
```

--------------------------------------------------------

```
[share]         　　　　　　　　　　　　　　　　　　　　　#自定义共享名称
comment =  This is share software       　　　　　　 #共享描述
path  =  /home/testfile                            #共享目录路径
browseable  =  yes/no                      　　　　　#设置共享是否可浏览，如果no就表示隐藏，需要通过IP+共享名称进行访问
writable  =  yes/no  　　　　　　　　　　　　　　　　　　#设置共享是否具有可写权限
read only  =  yes/no  　　　　　　　　　　　　　　　　　#设置共享是否具有只读权限
admin users  =  root 　　　　　　　　　　　　　　　　　　#设置共享的管理员，如果security =share 时，引项无效，多用户中间使用逗号隔开，例如admin users = root,user1,user2
valid users  =  username  　　　　　　　　　　　　　　　#设置允许访问共享的用户，例如valid users = user1,user2,@group1,@group2（多用户或组使用逗号隔开，@group表示group用户组）
invalid users  =  username     　　　　　　　　　　　　#设置不允许访问共享的用户
write list  =  username   　　　　　　　　　　　　　　　#设置在共享具有写入权限的用户，例如例如write list  = user1,user2,@group1,@group2（多用户或组使用逗号隔开，@group表示group用户组）
public  =  yes/no   　　　　　　　　　　　　　　　　　　　#设置共享是否允许guest账户访问
guest  ok  =  yes/no  　　　　　　　　　　　　　　　　　　#功能同public 一样
create mask = 0700                  　　　　　　　　　#创建的文件权限为700
directory mode = 0700               　　　　　　　　　#创建的文件目录为 700
```

参考文章：《[samba服务配置（一）](https://www.cnblogs.com/zoulongbin/p/7216246.html)》

​				   《[在Debian 8上安装Samba服务器](https://www.howtoing.com/debian-samba-server)》

