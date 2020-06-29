### String类型
string是redis最基本的类型。其数据结构是简单的key-value类型。value不仅是string，也可以是数字，是包含很多种类型的特殊类型。

string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象。

string类型是Redis最基本的数据类型，string类型的值最大能存储512MB。

#### string应用场景
1. String通常用于保存单个字符串或json字符串数据

2. 因String是二进制安全的，所以你完全可以把一个图片文件的内容作为字符串来存储

3. 计数器（常规key-value缓存应用。常规计数：微博数，粉丝数）

4. 分布式锁。在一个集群环境下，多个web应用时对同一个商品进行抢购和减库存操作时，可能出现超卖时会用到分布式锁

>INCR等指令本身就具有原子操作的特性，所以我们完全可以利用redis的INCR、INCRBY、DECR、DECRBY等指令来实现原子计数的效果。
