### AOP 常见使用场景

#### 日志场景
- 诊断上下文，如：log4j 或 logback 中的 _x0008_MDC
- 辅助信息，如：方法执行时间

#### 统计场景
- 方法调用次数
- 执行异常次数
- 数据抽样
- 数值累加

#### 安防场景
- 熔断，如：Netflix Hystrix
- 限流和降级：如：Alibaba Sentinel
- 认证和授权，如：Spring Security
- 监控，如：JMX

#### 性能场景
- 缓存，如 Spring Cache
- 超时控制
