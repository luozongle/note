[TOC]

Linux自定义service



如果我们想在linux服务器上配置redis自启动，最好的方式就是配置service



在`/usr/lib/systemd/system`目录下创建redis.service文件
```service
[Unit]
Description=Redis daemon
After=network.target

[Service]
Type=forking
ExecStart=/root/software/redis-3.2.10/bin/redis-server /root/software/redis-3.2.10/redis.conf
ExecStop=/root/software/redis-3.2.10/bin/redis-cli SHUTDOWN


[Install]
WantedBy=multi-user.target
```



```shell
systemctl start redis  # 启动redis
systemctl enable redis # 设置redis开机自启动
```



service文件说明:

- \[Unit\]: unit本身的说明,以及与其他相依daemon的设定，包括在什么服务之后才启动此unit之类的设定值
- \[Service\],\[Socket\],\[Timer\]....:不同的unit type就得要使用相对应的设定项目。
- \[Install\]:这个项目就是将此unit安装到哪个target









