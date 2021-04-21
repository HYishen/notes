CommitFailedException：Consumer客户端在提交位移时出现了错误或异常，而且还是那种不可恢复的严重异常

CommitFailedException异常的典型场景：当消息处理的总时间超过预设的max.poll.interval.ms参数值时，Kafka Consumer端会抛出CommitFailedException异常。

预防CommitFailedException异常的4种方法：

1. 缩短单条消息的处理时间；
2. 增加Consumer端允许下游系统消费一批消息的最大市场；
3. 减少下游系统一次性消费的消息总数；
4. 下游系统使用多线程来加速消费；
