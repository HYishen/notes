https://time.geekbang.org/column/article/68633

https://time.geekbang.org/column/article/76446

binlog 是逻辑日志，记录的是这个语句的原始逻辑

binlog 是可以追加写入的。“追加写”是指 binlog 文件写到一定大小后会切换到下一个，并不会覆盖以前的日志

### binlog 的三种格式
- statement
- row
- mixed
