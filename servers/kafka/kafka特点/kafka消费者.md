### 消费方式
consumer采用pull（拉）模式从broker中读取数据。

push（推）模式很难适应消费速率不同的消费者，因为消息发送速率是有broker来决定的。它的目标是尽可能以最快速度传递消息，但是这样很容易造成consumer来不及处理消息，典型的表现就是拒绝服务以及网络拥堵。而pull模式则可以根据consumer的消费能力以适当的速率消费消息。

pull模式不足之处是，如果kafka没有数据，消费者可能会陷入循环中，一直返回空数据。针对这一点，kafka的消费者在消费数据时会传入一个时长参数timeout，如果当前没有数据可供消费，consumer会等待一段时间之后再返回，这段时长即为timeout。

### 分区分配策略
一个consumer group中有多个consumer，一个topic有多个partition，所以必然会涉及到partition的分配问题，即确定那个partition由哪个consumer来消费。

kafka有两种分配策略，一是RoundRobin，一是Range。

当consumer group里面的consumer数量发生变化时，就会出发分区分配策略。

#### RoundRobin
把所有的topic当作一个整体，这些topic的所有partition以轮循的方式一个一个地分配给consumer group。

#### Range
把一个topic当作一个整体，这些topic中的partition以轮循的方式一个一个地分配给consumer group。不过这种策略有个问题是，在多个topic的partition进行分配之后，有一些consumer group中的consumer会得到比较多个partition。

### offset的维护
由于consumer在消费过程中会出现断电宕机等故障，consumer恢复后，需要从故障前的位置继续消费，所以consumer需要实时记录自己消费到了哪个offset，一遍故障恢复后继续消费。

offset是由consumer group来记录的，假如说consumer group中的一个consumer挂掉了，那么其他consumer可以继续根据offset来进行消费。

offset是有group + topic + partition来唯一决定的

kafka0.9版本之前，consumer默认将offset保存在zookeeper中，从0.9版本开始，consumer默认将offset保存在kafka一个内置的topic中，该topic为__consumer_offsets。

要读取__consumer_offsets需要设置consumer.properties
```
// 消费者不排除内部主题，即让消费者能够消费内部主题
exclude.internal.topics=false
```
