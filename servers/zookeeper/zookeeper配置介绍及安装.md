### 配置参数解读
Zookeeper中的配置文件zoo.cfg中参数含义：

1. tickTime=2000：通信心跳数，Zookeeper服务器与客户端心跳时间，单位毫秒

Zookeeper使用的基本时间，服务器之间或客户端与服务器之间维持心跳的时间间隔，也就是每个tickTime时间就会发送一个心跳，时间单位为毫秒。

它用于心跳机制，并且设置最小的session超时时间为两倍心跳时间。（session的最小超时时间为2×tickTime）

2. initLimit=10：Leader和Follower初始通信时限

集群中的Follower跟随者服务器与Leader领导者服务器之间初始连接时能容忍的最多心跳数（tickTime的数量），用它来限定集群中的Zookeeper服务器连接到Leader的时限。

3. syncLimit=5：Leader和Follower同步通信时限

集群中Leader与Follower之间的最大响应时间单位，假如响应时间超过syncLimit×tickTime，Leader认为Follower死掉，从服务器列表中删除Follower。

4. dataDir：数据文件目录+数据持久化路径

主要用于保存Zookeeper中的数据。

5. clientPort=2181：客户端连接端口

监听客户端连接的端口。

### 启动配置
1. 将zookeeper的conf目录下的zoo_sample.cfg拷贝一份成zoo.cfg文件
2. 打开zoo.cfg文件，修改dataDir路径，路径根据情况自己配置

### 操作zookeeper
1. 启动zookeeper：bin/zkServer.sh start
2. 查看进程是否启动：命令：jps
3. 查看状态：bin/zkServer.sh status
4. 启动客户端：bin/zkCli.sh
5. 退出客户端：quit
6. 停止zookeeper：bin/zhServer.sh stop

### 分布式安装部署
#### 配置服务器编号
在dataDir目录上（如：zookeeper/zkData）创建一个myid的文件，然后在文件中添加与server对应的编号（如：2）
```
touch myid
vim myid //然后在文件里面写入 “2”

// 然后同步分发到其他机器上，然后再改其他机器的myid文件
xsync myid
```

#### 给各个机器的zoo.cfg文件添加集群配置信息
```
############ cluster ############
server.2=zk02:2888:3888
server.3=zk03:2888:3888
server.4=zk04:2888:3888
```

同步zoo.cfg配置文件
```
xsync zoo.cfg
```

配置参数解读：
```
server.A=B:C:D
```

A是一个数字，表示这个是第几号服务器；集群模式下配置一个文件myid，这个文件在dataDir目录下，这个文件里面有一个数据就是A的值，**Zookeeper启动时读取次文件，拿到里面的数据与zoo.cfg里面的配置信息比较从而判断到底是哪个server**。

B是这个服务器的ip地址；

C是这个服务器与集群中的Leader服务器交换信息的端口；

D是万一集群中的Leader服务器挂了，需要一个端口来重新进行选举，选出一个新的Leader，而这个端口就是用来执行选举时服务器相互通信的端口。
