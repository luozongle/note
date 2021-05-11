```shell
tar -xvf kafka_2.11-2.3.0.tgz
```





```properties
broker.id=0   # 每个broker都不能相同 也就是每个kafka实例的broker.id都需要不一样
log.dirs=/data/test/kafka1/logs  
zookeeper.connect=localhost:8951,localhost:8952,localhost:8953
listeners=PLAINTEXT://10.153.106.14:8963
```

 

启动kafka   

```shell
kafka1/bin/kafka-server-start.sh -daemon kafka1/config/server.properties
```

