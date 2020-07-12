### Redis 事务
Redis 事务可以一次执行多个命令， 并且带有以下三个重要的保证：

- 批量操作在发送 EXEC 命令前被放入队列缓存。
- 收到 EXEC 命令后进入事务执行，事务中任意命令执行失败，其余的命令依然被执行。
- 在事务执行过程，其他客户端提交的命令请求不会插入到事务执行命令序列中。

一个事务从开始到执行会经历以下三个阶段：

- 开始事务。
- 命令入队。
- 执行事务。

### Redis事务中的错误处理
如果一个事务中的某个命令执行出错，Redis的处理方式根据错误类型的不同也产生不同

#### 1.语法错误
```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> APPEND
127.0.0.1:6379> SET key value
QUEUED
127.0.0.1:6379> SET key
(error) ERR wrong number of arguments for 'set' command
127.0.0.1:6379> SOMECOMMAND key
(error) ERR unknown command 'somecommand'
127.0.0.1:6379> EXEC
(error) EXECABORT Transaction discarded because of previous errors.
127.0.0.1:6379>
```
在mutil命令后执行了3个命令，一个是正确的，然后被加入了QUEUED中，其余都有错误，只要有一个命令语法错误，执行EXEC命令后Redis就会直接返回错误，正确的命令也不会执行（Redis 2.6.5之后）

#### 2.运行错误
运行错误指在命令执行时出现的错误。比如散列类型的命令操作集合类型的键，这种错误在执行之前Redis是无法发现的，所以在事务里这样的命令是会被Redis接受并执行的。
```
127.0.0.1:6379> MULTI
OK
127.0.0.1:6379> SET key 1
QUEUED
127.0.0.1:6379> SADD key 2
QUEUED
127.0.0.1:6379> SET key 3
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) (error) WRONGTYPE Operation against a key holding the wrong kind of value
3) OK
127.0.1:6379>
```
由于之前已经设置key为字符串类型，当使用SADD命令时起了冲突，但是，命令的语法并没有错误，是可以执行的，在执行之后才能发现错误。所以，SET key 3 仍然可以执行。但是请注意，redis不支持回滚。
