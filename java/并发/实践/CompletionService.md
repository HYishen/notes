CompletionService 的实现原理是内部维护了一个阻塞队列，当任务执行结束就把任务的执行结果加入到阻塞队列中

CompletionService 能够让异步任务的执行结果有序化，先执行完的先进入阻塞队列，利用这个特性，你可以轻松实现后续处理的有序性，避免无谓的等待

### 创建 CompletionService
CompletionService 接口的实现类是 ExecutorCompletionService，这个实现类的构造方法有两个，分别是：

1. ExecutorCompletionService(Executor executor)；
2. ExecutorCompletionService(Executor executor, BlockingQueue> completionQueue)。

这两个构造方法都需要传入一个线程池，如果不指定 completionQueue，那么默认会使用无界的 LinkedBlockingQueue。任务执行结果的 Future 对象就是加入到 completionQueue 中。

下面的示例代码完整地展示了如何利用 CompletionService 来实现高性能的询价系统。其中，我们没有指定 completionQueue，因此默认使用无界的 LinkedBlockingQueue。之后通过 CompletionService 接口提供的 submit() 方法提交了三个询价操作，这三个询价操作将会被 CompletionService 异步执行。最后，我们通过 CompletionService 接口提供的 take() 方法获取一个 Future 对象（前面我们提到过，加入到阻塞队列中的是任务执行结果的 Future 对象），调用 Future 对象的 get() 方法就能返回询价操作的执行结果了。

```
// 创建线程池
ExecutorService executor = 
  Executors.newFixedThreadPool(3);
// 创建CompletionService
CompletionService<Integer> cs = new 
  ExecutorCompletionService<>(executor);
// 异步向电商S1询价
cs.submit(()->getPriceByS1());
// 异步向电商S2询价
cs.submit(()->getPriceByS2());
// 异步向电商S3询价
cs.submit(()->getPriceByS3());
// 将询价结果异步保存到数据库
for (int i=0; i<3; i++) {
  Integer r = cs.take().get();
  executor.execute(()->save(r));
}
```

### CompletionService 接口说明
下面我们详细地介绍一下 CompletionService 接口提供的方法，CompletionService 接口提供的方法有 5 个，这 5 个方法的方法签名如下所示。

```
Future<V> submit(Callable<V> task);
Future<V> submit(Runnable task, V result);
Future<V> take() 
  throws InterruptedException;
Future<V> poll();
Future<V> poll(long timeout, TimeUnit unit) 
  throws InterruptedException;
```

其中，submit() 相关的方法有两个。一个方法参数是Callable task，前面利用 CompletionService 实现询价系统的示例代码中，我们提交任务就是用的它。另外一个方法有两个参数，分别是Runnable task和V result，这个方法类似于 ThreadPoolExecutor 的 Future submit(Runnable task, T result)

CompletionService 接口其余的 3 个方法，都是和阻塞队列相关的，take()、poll() 都是从阻塞队列中获取并移除一个元素；它们的区别在于如果阻塞队列是空的，那么调用 take() 方法的线程会被阻塞，而 poll() 方法会返回 null 值。 poll(long timeout, TimeUnit unit) 方法支持以超时的方式获取并移除阻塞队列头部的一个元素，如果等待了 timeout unit 时间，阻塞队列还是空的，那么该方法会返回 null 值。
