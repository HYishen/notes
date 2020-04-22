使用文件：bin/kafka-topics.sh

#### 查看当前服务器中的所有topic
```
bin/kafka-topics.sh --zookeeper [zookeeper host:port] --list
```

#### 创建topic
```
bin/kafka-topics.sh --zookeeper [zookeeper host:port] --create --replication-factor [number] --partitions [number] --topic [topic name]

选项说明：
--topic    定义topic名
--replication-factor    定义副本数
--partitions    定义分区数
```

#### 删除topic
```
bin/kafka-topics.sh --zookeeper [zookeeper host:port] --delete --topic [topic name]
```
> 需要再server.properties中设置delete.topic.enable=true 否则只是标记删除

#### 发送消息
```
bin/kafka-console-producer.sh --broker-list [kafka host:port] --topic [topic name]
```

#### 消费消息
```
// 旧版本消费者
bin/kafka-console-consumer.sh --zookeeper [zookeeper host:port] --topic [topic name]

// 新版本消费者
bin/kafka-console-consumer.sh --bootstrap-server [kafka host:port] --topic [topic name]

bin/kafka-console-consumer.sh --bootstrap-server [kafka host:port] --from-begining --topic [topic name]
```

> --from-begining：会把主题中以往所有的数据都读取出来

#### 查看某个topic的详情
```
bin/kafka-topics.sh --zookeeper [zookeeper host:port] --describe --topic [topic name]
```

#### 修改分区数
```
bin/kafka-topics.sh --zookeeper [zookeeper host:port] --alter --topic [topic name] --partitions [number]
```