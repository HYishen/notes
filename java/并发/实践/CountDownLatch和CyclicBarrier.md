### 用 CountDownLatch 实现线程等待
下面的代码示例中，在 while 循环里面，我们首先创建了一个 CountDownLatch，计数器的初始值等于 2，之后在pos = getPOrders();和dos = getDOrders();两条语句的后面对计数器执行减 1 操作，这个对计数器减 1 的操作是通过调用 latch.countDown(); 来实现的。在主线程中，我们通过调用 latch.await() 来实现对计数器等于 0 的等待。
```
// 创建2个线程的线程池
Executor executor = 
  Executors.newFixedThreadPool(2);
while(存在未对账订单){
  // 计数器初始化为2
  CountDownLatch latch = 
    new CountDownLatch(2);
  // 查询未对账订单
  executor.execute(()-> {
    pos = getPOrders();
    latch.countDown();
  });
  // 查询派送单
  executor.execute(()-> {
    dos = getDOrders();
    latch.countDown();
  });
  
  // 等待两个查询操作结束
  latch.await();
  
  // 执行对账操作
  diff = check(pos, dos);
  // 差异写入差异库
  save(diff);
}
```

### 用 CyclicBarrier 实现线程同步
在下面的代码中，我们首先创建了一个计数器初始值为 2 的 CyclicBarrier，你需要注意的是创建 CyclicBarrier 的时候，我们还传入了一个回调函数，当计数器减到 0 的时候，会调用这个回调函数。

线程 T1 负责查询订单，当查出一条时，调用 barrier.await() 来将计数器减 1，同时等待计数器变成 0；线程 T2 负责查询派送单，当查出一条时，也调用 barrier.await() 来将计数器减 1，同时等待计数器变成 0；当 T1 和 T2 都调用 barrier.await() 的时候，计数器会减到 0，此时 T1 和 T2 就可以执行下一条语句了，同时会调用 barrier 的回调函数来执行对账操作。

非常值得一提的是，**CyclicBarrier 的计数器有自动重置的功能，当减到 0 的时候，会自动重置你设置的初始值**。这个功能用起来实在是太方便了。

```
// 订单队列
Vector<P> pos;
// 派送单队列
Vector<D> dos;
// 执行回调的线程池 
Executor executor = 
  Executors.newFixedThreadPool(1);
final CyclicBarrier barrier =
  new CyclicBarrier(2, ()->{
    executor.execute(()->check());
  });
  
void check(){
  P p = pos.remove(0);
  D d = dos.remove(0);
  // 执行对账操作
  diff = check(p, d);
  // 差异写入差异库
  save(diff);
}
  
void checkAll(){
  // 循环查询订单库
  Thread T1 = new Thread(()->{
    while(存在未对账订单){
      // 查询订单库
      pos.add(getPOrders());
      // 等待
      barrier.await();
    }
  });
  T1.start();  
  // 循环查询运单库
  Thread T2 = new Thread(()->{
    while(存在未对账订单){
      // 查询运单库
      dos.add(getDOrders());
      // 等待
      barrier.await();
    }
  });
  T2.start();
}
```

### 总结
CountDownLatch 和 CyclicBarrier 是 Java 并发包提供的两个非常易用的线程同步工具类，这两个工具类用法的区别在这里还是有必要再强调一下：

- CountDownLatch 主要用来解决一个线程等待多个线程的场景

- 而 CyclicBarrier 是一组线程之间互相等待，CyclicBarrier 的计数器是可以循环利用的，而且具备自动重置的功能，一旦计数器减到 0 会自动重置到你设置的初始值。除此之外，CyclicBarrier 还可以设置回调函数，可以说是功能丰富。
