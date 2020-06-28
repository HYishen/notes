### 键值设计
#### key名设计
##### 1.【建议】: 可读性和可管理性
以业务名(或数据库名)为前缀(防止key冲突)，用冒号分隔，比如业务名:表名:id
```
ugc:video:1
```
##### 2.【建议】：简洁性
保证语义的前提下，控制key的长度，当key较多时，内存占用也不容忽视，例如：
```
user:{uid}:friends:messages:{mid}简化为u:{uid}:fr:m:{mid}。
```
##### 3.【强制】：不要包含特殊字符
反例：包含空格、换行、单双引号以及其他转义字符

#### value设计
##### 1.【强制】：拒绝bigkey(防止网卡流量、慢查询)
string类型控制在10KB以内，hash、list、set、zset元素个数不要超过5000。

反例：一个包含200万个元素的list。

非字符串的bigkey，不要使用del删除，使用hscan、sscan、zscan方式渐进式删除，同时要注意防止bigkey过期时间自动删除问题(例如一个200万的zset设置1小时过期，会触发del操作，造成阻塞，而且该操作不会不出现在慢查询中(latency可查))，查找方法和删除方法

##### 2.【推荐】：选择适合的数据类型。
例如：实体类型(要合理控制和使用数据结构内存编码优化配置,例如ziplist，但也要注意节省内存和性能之间的平衡)

反例：
```
set user:1:name tom
set user:1:age 19
set user:1:favor football
```

正例:
```
hmset user:1 name tom age 19 favor football
```

##### 3.【推荐】：控制key的生命周期，redis不是垃圾桶。
建议使用expire设置过期时间(条件允许可以打散过期时间，防止集中过期)，不过期的数据重点关注idletime。
