#### Kafka基础概念
1. Producer： 消息生产者，就是向kafka broker发消息的客户端
2. Consumer： 消息消费者，向kafka broker取消息的客户端、
3. Consumer Group（CG）： 消费者组，由多个consumer组成。*消费者组内每个消费者负责消费不同分区的数据，一个分区只能由一个组内消费者消费；消费者组之间互不影响。*所有的消费者都属于某个消费者组，即*消费者组是逻辑上的一个订阅者*。
4. Broker： 一台kafka服务器就是一个broker。一个集群由多个broker组成。一个broker可以容纳多个topic。
5. Topic(主题): 可以理解为一个队列，生产者和消费者面向的都是一个topic；
6. Partition(分区)： 为了实现扩展性，一个非常大的topic可以分布到多个broker（即服务器）上，一个topic可以分为多个partition，每个partition是一个有序的队列；
7. Replica： 副本，为保证集群中的某个节点发生故障时，该节点上的partition数据不丢失，且kafka仍然能够工作，kafka提供了副本机制，一个topic的每个分区都有若干个副本，一个leader和若干个follower。
8. leader： 每个分区多个副本的“主”，生产者发送数据的对象，以及消费者消费数据的对象都是leader。
9. follower： 每个分区多个副本中的“从”，实时从leader中同步数据，保持和leader数据的同步。leader发生故障时，某个follower会成为新的leader。
10. Offset(位移):表示分区中每条消息的位置信息，是一个单调递增且不变的值。
11. Record(消息):消息就是指 Kafka 处理的主要对象

#### Kafka的三层架构

- 第一层是主题层，每个主题可以配置 M 个分区，而每个分区又可以配置 N 个副本。

- 第二层是分区层，每个分区的 N 个副本中只能有一个充当领导者角色，对外提供服务；其他 N-1 个副本是追随者副本，只是提供数据冗余之用。

- 第三层是消息层，分区中包含若干条消息，每条消息的位移从 0 开始，依次递增。

最后，客户端程序只能与分区的领导者副本进行交互。

