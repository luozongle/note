[TOC]

# FTP安装文档

## 一、简介



常用的ftp、tftp服务器软件:FileZilla Server、Wing FTP Server、Serv-U、vsftpd、tftp



安装vsftpd(Very Secure FTP Daemon)

```shell
# 检查是否安装了vsftpd
rpm -qa | grep vsftpd

# 如果没有安装则安装一下
yum install -y vsftpd

systemctl start vsftpd.service
```



安装后生成的文件

1.用户控制列表文件

```file
/etc/vsftpd/ftpusers
/etc/vsftpd/user_list
```



2.主配置文件

```file
/etc/vsftpd/vsftpd.conf
```



3.ftp用户默认工作目录(宿主目录)

```file
/var/ftp
```



4.ftp用户默认登录的目录

```file
/var/ftp/pub
```





用户类型

- 本地用户(local):用户在ftp服务器拥有账号，且该账号为本地用户的账号，可以通过自己账号和口令进行授权登录，登录目录为自己的$HOME目录。不加(chroot_local_user=YES)的话本地用户可以移动到服务器的根目录，所以不推荐，不安全。本地用户登录之后是直接登录到该账号自己本身的默认工作目录的，如果该账户没有默认的工作目录，那么该账号不能通过ftp登录。
- 虚拟用户(guest):用户在ftp服务器上拥有账号，但该赵勇只能用于文件传输服务。登录目录为某一特定的目录，通常可以上传和下载。
- 匿名用户(anonymous):用户在ftp服务器上没有账号，登录目录为/var/ftp



连接模式

ftp一般是有两个连接的，一个是客户端给服务器传输命令的，另一个是数据传送的连接。FTP服务程序一般会支持两种不同的模式，一种是主动(Port)模式，一种是被动(Passive)模式(又称Pasv Mode)

- 主动(Port)模式:(服务端从20端口主动向客户端发起连接)
  - 当客户端C向服务器S连接后，使用的是Port模式，那么客户端C会发送一条命令告诉服务器端S(客户端C在本地打开了一个端口N在等着你进行数据连接)，当服务器S接收到这个Port命令后就会向客户端打卡的那个端口N进行连接，这种数据连接就生成了。
- 被动(Pasv)模式:(服务端在指定范围内某个端口被动等待客户端连接)
  - 当客户端C向服务端S连接后，服务端S会发送信息给客户端C，这个信息是(服务端S在本地打开了一个端口M)，当客户端C收到这个信息后，就可以向服务端S的M端口进行连接，连接成功后，数据连接也建立了。

> 两种模式的主要不同是数据连接建立的不同，对于port模式，是客户端C在本地打开一个窗口等服务端Squier连接建立数据连接;而Pasv模式就是服务端S打开一个端口等待客户端C去建立一个数据连接。



/etc/vsftpd/vsftpd.conf 配置文件详解

```conf
#开启匿名登录(使用ftp|FTP|anonymous用户登录，不用输入密码)
anonymous_enable=YES
# 允许使用本地账户进行FTP用户登录验证
local_enable=YES
# 允许写
write_enable=YES
#设置本地用户默认文件掩码022
local_umask=022
# 允许匿名上传
anon_upload_enable=YES
# 允许匿名创建新目录
anon_mkdir_write_enable=YES
# 允许匿名用户删除目录
anon_other_write_enable=YES
# 可以发送消息当访问某个目录时
dirmessage_enable=YES
# 开启上传下载记录
xferlog_enable=YES
# 数据连接通过20端口建立
connect_from_port_20=YES

# 允许其它用户上传匿名文件
chown_uploads=YES
# 所有用户
chown_username=whoever
# 日志标准输出
xferlog_std_format=YES
# 空闲会话时间
idle_session_timeout=600
# 数据连接超时时间
data_connection_timeout=120
# 隔离的安全用户
nopriv_user=ftpsecure
# 开启异步数据线程
async_abor_enable=YES
# 开启ASCII协议上传
ascii_upload_enable=YES
# 开启ASCII协议下载
ascii_download_enable=YES
# 开启邮箱验证
deny_email_enable=YES
# 拒绝的邮箱列表
banned_email_file=/etc/vsftpd/banned_emails


# 是否允许直接获取子目录信息
ls_recure_enable=YES
# 监听ipv4
listen=NO
# 监听ipv6和监听ipv4
listen_ipv6=YES

# 虚拟用户启用pam认证
pam_service_name=vsftpd
# 用户组管理
userlist_enable=YES
# 访问控制
tcp_wrappers=YES

# 允许使用被动模式
pasv_enable=YES
# 指定使用被动模式时打开端口的最小值
pasv_min_port=10060
# 指定使用被动模式时打开端口的最大值
pasv_max_port=10090
# 用户带宽限制200kps
local_max_rate=200000
# 登录后欢迎内容
ftpd_banner=Welcome to My FTP service.

# ---开启虚拟用户组参数-----
# 开启虚拟用户
guest_enable=YES
# 主虚拟用户名vsftpd，等下会建立
guest_username=vsftpd
# 虚拟用户配置（可以对每一个虚拟用户进行单独的权限配置）
user_config_dir=/etc/vsftpd/vconf

# 启用限定用户在其主目录下
chroot_local_user=YES
# 开启用户列表chroot管理
chroot_list_enable=YES
# chroot管理的用户列表（一行一用户,虚拟用户都要添加进去）
# 当设置用户只能在登录目录时，chroot管理的用户为不受限制，否则相反
chroot_list_file=/etc/vsftpd/chroot_list
# 允许chroot管理用户进行写操作
allow_writeable_chroot=YES

# ---------虚拟用户高级参数（请选择一组）--------
# 虚拟用户和本地用户有相同的权限
virtual_use_local_privs=YES

# 虚拟用户和匿名用户有相同的权限，默认是NO
virtual_use_local_privs=NO

# 虚拟用户具有写权限（上传、下载、删除、重命名）
virtual_use_local_privs=YES
write_enable=YES

# 虚拟用户不能浏览目录，只能上传文件，无其他权限
virtual_use_local_privs=NO
write_enable=YES
anon_world_readable_only=YES
anon_upload_enable=YES

# 虚拟用户只能下载文件，无其他权限
virtual_use_local_privs=NO
write_enable=YES
anon_world_readable_only=NO
anon_upload_enable=NO

# 虚拟用户只能上传和下载文件，无其他权限
virtual_use_local_privs=NO
write_enable=YES
anon_world_readable_only=NO
anon_upload_enable=YES

# 虚拟用户只能下载文件和创建文件夹，无其他权限
virtual_use_local_privs=NO
write_enable=YES
anon_world_readable_only=NO
anon_mkdir_write_enable=YES

# 虚拟用户只能下载、删除和重命名文件，无其他权限
virtual_use_local_privs=NO
write_enable=YES
anon_world_readable_only=NO
anon_other_write_enable=YES
```







常用命令

```
ls # 查看远程服务器目录
!ls # 查看本地客户端目录
cd # 切换远程目录
!cd # 切换本地目录
pwd # 切换远程目录
!pwd # 切换本地目录
delete # 删除远程目录文件
!rm # 删除远程目录
```





常见问题

1.配置匿名用户如果上传文件出现553，需要把对应目录改成777权限