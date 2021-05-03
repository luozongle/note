[TOC]

# Redis安装文档



## 一、Installing Redis on macOS

环境:

系统版本:macOS Big Sur 11.2.2

redis版本:redis-6.2.2



### 1.下载、解压、编译redis

```shell
wget https://download.redis.io/releases/redis-6.2.2.tar.gz
tar xzf redis-6.2.2.tar.gz
cd redis-6.2.2
make
```



### 2.修改redis配置文件

```config
daemonize yes  # 后台启动  
protected-mode no # 关闭保护模式  
#bind 127.0.0.1  (bind绑定的是自己机器网卡的ip，如果有多块网卡可以配多个ip，代表允许客户端通过机器的哪些网卡ip去访问，内网一般可以不配置bind，注释掉即可)  

maxmemory 512MB  # 配置Redis存储数据时指定限制的内存大小，当缓存消耗的内存超过这个数值时，将触发数据淘汰。  
maxmemory-policy noeviction
maxmemory-samples 5

pidfile /opt/redis-6.2.2/redis_6379.pid 指定pid文件地址
logfile "/opt/redis-6.2.2/log/redis.log" #指定日志文件位置,不指定不会输出日志，日志目录需要手动创建

syslog-enabled yes

dbfilename redis_dump.rdb
dir /opt/redis-6.2.2/data/
```





## 二、Installing Redis on Centos