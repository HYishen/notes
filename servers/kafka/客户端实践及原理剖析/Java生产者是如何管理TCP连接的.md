Kafka社区决定采用TCP而不是HTTP作为所有请求通信的底层协议的原因：在开发客户端时，人们能够利用TCP本身提供的一些高级高级功能，比如多路复用请求以及同时轮询多个连接的能力；目前已知的HTTP库在很多编程语言中都略显简陋。

针对TCP连接何时创建的问题，目前我们的结论是TCP连接是在创建KafkaProducer实例时建立的。但是TCP连接还可能在两个地方被创建，一个是在更新元数据后，另一个是在消息发送时。
```
1.KafkaProducer 实例创建时启动 Sender 线程，从而创建与 bootstrap.servers 中所有 Broker 的 TCP 连接。

2.KafkaProducer 实例首次更新元数据信息之后，还会再次创建与集群中所有 Broker 的 TCP 连接。

3.如果 Producer 端发送消息到某台 Broker 时发现没有与该 Broker 的 TCP 连接，那么也会立即创建连接。
```

Producer端关闭TCP连接的方式有两种：一种是用户主动关闭；一种是Kafka自动关闭
```
// 1 主动关闭
方法一：调用producer.close()方法
方法二：直接执行kill -9等主动杀掉Producer应用的方法

// 2 自动关闭
如果设置 Producer 端 connections.max.idle.ms 参数大于 0，则步骤 1 中创建的 TCP 连接会被自动关闭；如果设置该参数 =-1，那么步骤 1 中创建的 TCP 连接将无法被关闭，从而成为“僵尸”连接。
```