https://time.geekbang.org/column/article/68633

redo log是innodb引擎特有的日志，是在引擎层的

redo log 是物理日志，记录的是“在某个数据页上做了什么修改”

redo log 是循环写的，空间固定会用完；
