### 分区策略
#### 分区原因
（1）方便在集群中扩展，每个partition可以通过调整以适应它所在的机器，而每一个topic又可以有多个partition组成，因此整个集群就可以适应任意大小的数据了；

（2）可以提高并发，因为可以以partition为单位读写。

#### 分区规则
在API中我们需要将producer发送的数据封装成一个ProducerRecord对象。

（1）指明partition的情况下，直接将指明的值直接作为partition值；

（2）没有指明partition值，但有key的情况下，将key的hash值与topic的partition数据进行取余得到partition值；

（3）即没有partition值，又没有key值的情况下，第一次调用时随机生成一个整数（后面每次调用在这个整数上自增），将这个值与topic可用的partition总数取余得到partition值，也就是常说的round-robin算法。

### 保证数据可靠性
为保证producer发送的数据，能可靠的发送到指定的topic，topic每个partition收到producer发送的数据后，都需要向producer发送ack（acknowledgement确认收到），如果producer收到ack，就会进行下一轮的发送，否则重新发送数据。

#### 副本数据同步策略
一般情况下有两种数据同步策略
1. 半数以上完成同步，就会发送ack，优点：延迟低，缺点：选举新的leader时，容忍n台节点的故障，需要2n+1个副本
2. 全部完成同步，才发送ack，优点：选举新的leader时，容忍n台节点的故障，需要n+1个副本，缺点：延迟高

kafka选择了第二种方案，原因：
1. 同样为了容忍n台节点的故障，第一种方案需要2n+1个副本，而第二种方案只需要n+1个副本，而kafka的每个分区都有大量的数据，第一种方案会造成大量数据的冗余。
2. 虽然第二种方案的网络延迟会比较高，但是网络延迟对kafka的影响较小。

#### ISR
采用第二种方案后，设想一下情景：leader收到数据，所有follower都开始同步数据，但有一个follower，因为某种故障，迟迟不能与leader进行同步，那么leader就要一直等下去，直到它同步完成，才能发送ack。这个问题怎么解决呢？

Leader维护了一个动态的in-sync replica set（ISR），意为和leader保持同步的follower集合。当ISR中的follower完成数据同步之后，leader就会给follower发送ack。如果follower长时间未向leader同步数据，则该follower将被踢出ISR，该时间阈值由replica.lag.time.max.ms参数设定。Leader发生故障之后，就会从ISR中选举新的leader。

#### ack应答机制
对于某些不太重要的数据，对数据的可靠性要求不是很高，能容忍数据的少量丢失，所以没必要等ISR中的follower全部接收成功。

所以kafka为用户提供了三种可靠性级别，用户根据对可靠性和延迟的要求进行权衡，选择以下的配置。

acks：

0：producer不等待broker的ack，这一操作提供了一个最低的延迟，broker一接收到还没有写入磁盘就已经返回，当broker故障时有可能丢失数据；

1：producer等待broker的ack，partition的leader落盘成功后返回ack，如果在follower同步成功之前leader故障，那么将会丢失数据；

-1（all）：producer等待broker的ack，partition的leader和follower全部落盘成功后才返回ack。但是如果在follower同步完成后，broker发送ack前，leader发送故障，那么会造成数据重复（因为新选举的leader已经有原来的数据，但是生产者没有收到ack，会重新发送数据）。

#### 故障处理细节
log文件中的HW（high watermark）和LEO（log end offset）

LEO：指的是每个副本最大的offset；

HW：指的是消费者能见到的最大offset，ISR队列中最小的LEO。

（1）follower故障

follower发生故障后会被临时踢出ISR，待该follower恢复后，follower会读取本地磁盘记录的上次的HW，并将log文件高于HW的部分截取掉，从HW开始向leader进行同步。等该follower的LEO大于等于该partition的HW，即follower追上leader之后，就可以重新加入ISR了。

（2）leader故障

leader发生故障之后，会从ISR中选出一个新的leader，之后，为保证多个副本之间的数据一致性，其余的follower会先将各自的log文件高于HW的部分截取掉，然后从新的leader同步数据。

> 注意：这只能保证副本之间的数据一致性，并不能保证数据不会丢失或者不重复。

### Exactly Once语义
将服务器的ack级别设置为-1，可以保证producer到Server之间不会丢失数据，即At Least Once语义。相对的，将服务器ack级别设置为0，可以保证生产者每条消息只会被发送一次，即At Most Once语义。

At Least Once可以保证数据不丢失。但是，对于一些非常重要的信息，比如说交易数据，下游数据消费者要求数据既不能重复也不能丢失，即Exactly Once语义。在0.11版本以前的kafka，对此是无能为力的，只能保证数据不丢失，再在下游消费者对数据做全局去重。对于多个下游应用的情况，每个都需要单独做全局去重，这就对性能造成了很大影响。

0.11版本的kafka，引入了一项重大特性：幂等性。所谓的幂等性就是指producer不论向Server发送多少次重复数据，Server端都只会持久化一条。幂等性结合At Least Once语义，就构成了kafka的Exactly Once语义。即：
```
Al Least Once + 幂等性 = Exactly Once
```
要启用幂等性，只需要将producer的参数中enable.idompotence设置为true即可。kafka的幂等性实现其实就是将原来下游需要做的去重放在了数据上游。开启幂等性的Producer在初始化的时候会分配一个PID，发往同一个Partition的消息会附带Sequence  Number。而Broker端会对\<PID,Partition,SeqNumber\>做缓存，当具有相同主键的消息提交时，Broker只会持久化一条。

但是PID重启就会变化，同时不同的partition也具有不同主键，所以幂等性无法保证跨分区跨回话的Exactly Once。