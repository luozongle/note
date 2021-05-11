下载需要的zk
```url
https://archive.apache.org/dist/zookeeper/
```





```shell
tar -xvf apache-zookeeper-3.5.5-bin.tar.gz
cp apache-zookeeper-3.5.5-bin/conf/zoo_sample.cfg apache-zookeeper-3.5.5-bin/conf/zoo.cfg
```



zoo.cfg配置

```conf
tickTime=2000
initLimit=10
syncLimit=5
dataDir=/data/test/zk1/data
clientPort=8951
server.1=127.0.0.1:8971:8981
server.2=127.0.0.1:8972:8882
server.3=127.0.0.1:8973:8983
```



需要创建zk的data目录，并且需要为每个节点在data目录下创建myid

```
echo 1 > /data/test/zk1/data/myid
echo 2 > /data/test/zk2/data/myid
echo 3 > /data/test/zk3/data/myid
```



```shell
/data/test/zk1/bin/zkServer.sh start
/data/test/zk2/bin/zkServer.sh start
/data/test/zk3/bin/zkServer.sh start
```

