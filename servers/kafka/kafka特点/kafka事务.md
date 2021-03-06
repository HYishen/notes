### kafka事务
kafka从0.11版本开始引入了事务支持。事务可以保证kafka在Exactly Once语义的基础上，生产和消费可以跨分区和会话，要么全部成功，要么全部失败。

#### producer事务
为了实现跨分区跨会话的事务，需要引入一个全局唯一的TransactionID，并将producer获得的PID和TransactionID绑定。这样当Producer重启后就可以通过正在进行的TransactionID获得原来的PID。

为了管理Transaction，kafka引入了一个新的组件Transaction Coordinator。producer就是通过Transaction Coordinator交互获得Transaction ID对应的任务状态。Transaction Coordinator还负责将事务所有写入kafka的一个内部topic，这样即使整个服务重启，由于事务状态得到保存，进行中的事务状态可以得到恢复，从而继续进行。

#### consumer事务
上述事务机制主要是从producer方面考虑，对于consumer而言，事务的保证就会相对较弱，尤其是无法保证commit的信息被精确消费。这是由于consumer可以通过offset访问任意信息，而且不同的segment file生命周期不同，同一事物的消息可能会出现重启后被删除的情况。