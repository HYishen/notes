使用文件：bin/kafka-topics.sh

查看当前服务器中的所有topic
```
bin/kafka-topics.sh --zookeeper [host:port] --list
```

创建topic
```
bin/kafka-topics.sh --zookeeper [host:port] --create --replication-factor [number] --partitions [number] --topic [topic name]

选项说明：
--topic    定义topic名
--replication-factor    定义副本数
--partitions    定义分区数
```
