### Consumer Group
**Consumer Group**是Kafka提供的可扩展且具有容错性的消费者机制

### Consumer Group的3个特征
1. Consumer Group下可以有一个或多个Consumer实例；

2. 在一个Kafka集群中，Group ID标识唯一的一个Consumer Group；

3. Consumer Group下所有的实例订阅的主题的单个分区，只能分配给组内的一个Consumer实例消费

Kafka仅仅使用Consumer Group这一种机制，却同时实现了传统消息引擎系统的两个大模型：如果所有实例都属于同一个Group，那么它的实现就是**消息队列模型**；如果所有实例都属于不同的Group，那么它的实现就是**发布/订阅模型**
