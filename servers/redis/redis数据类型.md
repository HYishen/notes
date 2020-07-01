### String类型
string是redis最基本的类型。其数据结构是简单的key-value类型。value不仅是string，也可以是数字，是包含很多种类型的特殊类型。

string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象。

string类型是Redis最基本的数据类型，string类型的值最大能存储512MB。

#### string应用场景
1. String通常用于保存单个字符串或json字符串数据

2. 因String是二进制安全的，所以你完全可以把一个图片文件的内容作为字符串来存储

3. 计数器（常规key-value缓存应用。常规计数：微博数，粉丝数）

4. 分布式锁。在一个集群环境下，多个web应用时对同一个商品进行抢购和减库存操作时，可能出现超卖时会用到分布式锁。使用SETNX命令（SET if Not eXists）

>INCR等指令本身就具有原子操作的特性，所以我们完全可以利用redis的INCR、INCRBY、DECR、DECRBY等指令来实现原子计数的效果。

### Hash类型
Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象。

可以看成具有key和value的map容器，该类型非常适合于存储值对象信息。

Redis 中每个 hash 可以存储 232 - 1 键值对（40多亿）。

#### Hash内部编码
哈希类型 的 内部编码 有两种：

##### ziplist（压缩列表）
当 哈希类型 元素个数 小于 hash-max-ziplist-entries 配置（默认 512 个）、同时 所有值 都 小于 hash-max-ziplist-value 配置（默认 64 字节）时，Redis 会使用 ziplist 作为 哈希 的 内部实现，ziplist 使用更加 紧凑的结构 实现多个元素的 连续存储，所以在 节省内存 方面比 hashtable 更加优秀。

##### hashtable（哈希表）
当 哈希类型 无法满足 ziplist 的条件时，Redis 会使用 hashtable 作为 哈希 的 内部实现，因为此时 ziplist 的 读写效率 会下降，而 hashtable 的读写 时间复杂度 为 O（1）。
