### 部署模式
#### Standalone模式

#### Yarn模式

 

### 部署相关命令
```
// 启动flink
./bin/start-cluster.sh

// 关闭flink
./bin/stop-cluster.sh

// 提交jar包
// example: ./bin/flink run -c flink.example.wc.SocketWordCount -p 4 /Users/yishen/develops/workspaces/FlinkTutorial/target/FlinkTutorial-1.0-SNAPSHOT.jar --host localhost --port 2333
./bin/flink run -c {main class} -p {parallelism} {jar path} {java arguments}

// 列出flink的task
./bin/flink list [-a]


// 取消flink的task
./bin/flink cancel {taskid}

```


### 部署配置

### 部署相关知识
堆外内存存放task状态信息
