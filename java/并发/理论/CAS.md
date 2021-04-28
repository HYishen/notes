### CAS
CPU 为了解决并发问题，提供了 CAS 指令（CAS，全称是 Compare And Swap，即“比较并交换”）。CAS 指令包含 3 个参数：共享变量的内存地址 A、用于比较的值 B 和共享变量的新值 C；并且只有当内存中地址 A 处的值等于 B 时，才能将内存中地址 A 处的值更新为新值 C。**作为一条 CPU 指令，CAS 指令本身是能够保证原子性的**。

你可以通过下面 CAS 指令的模拟代码来理解 CAS 的工作原理。在下面的模拟程序中有两个参数，一个是期望值 expect，另一个是需要写入的新值 newValue，只有当目前 count 的值和期望值 expect 相等时，才会将 count 更新为 newValue。

```
class SimulatedCAS{
  int count；
  synchronized int cas(
    int expect, int newValue){
    // 读目前count的值
    int curValue = count;
    // 比较目前count值是否==期望值
    if(curValue == expect){
      // 如果是，则更新count的值
      count = newValue;
    }
    // 返回写入前的值
    return curValue;
  }
}
```

**“只有当目前 count 的值和期望值 expect 相等时，才会将 count 更新为 newValue。”**

使用 CAS 来解决并发问题，一般都会伴随着自旋，而所谓自旋，其实就是循环尝试。例如，实现一个线程安全的count += 1操作，“CAS+ 自旋”的实现方案如下所示，首先计算 newValue = count+1，如果 cas(count,newValue) 返回的值不等于 count，则意味着线程在执行完代码①处之后，执行代码②处之前，count 的值被其他线程更新过。那此时该怎么处理呢？可以采用自旋方案，就像下面代码中展示的，可以重新读 count 最新的值来计算 newValue 并尝试再次更新，直到成功。

```
class SimulatedCAS{
  volatile int count;
  // 实现count+=1
  addOne(){
    do {
      newValue = count+1; //①
    }while(count !=
      cas(count,newValue) //②
  }
  // 模拟实现CAS，仅用来帮助理解
  synchronized int cas(
    int expect, int newValue){
    // 读目前count的值
    int curValue = count;
    // 比较目前count值是否==期望值
    if(curValue == expect){
      // 如果是，则更新count的值
      count= newValue;
    }
    // 返回写入前的值
    return curValue;
  }
}
```

通过上面的示例代码，想必你已经发现了，CAS 这种无锁方案，完全没有加锁、解锁操作，即便两个线程完全同时执行 addOne() 方法，也不会有线程被阻塞，所以相对于互斥锁方案来说，性能好了很多。

但是在 CAS 方案中，有一个问题可能会常被你忽略，那就是 ABA 的问题。什么是 ABA 问题呢？

前面我们提到“如果 cas(count,newValue) 返回的值不等于count，意味着线程在执行完代码①处之后，执行代码②处之前，count 的值被其他线程更新过”，那如果 cas(count,newValue) 返回的值等于count，是否就能够认为 count 的值没有被其他线程更新过呢？显然不是的，假设 count 原本是 A，线程 T1 在执行完代码①处之后，执行代码②处之前，有可能 count 被线程 T2 更新成了 B，之后又被 T3 更新回了 A，这样线程 T1 虽然看到的一直是 A，但是其实已经被其他线程更新过了，这就是 ABA 问题。

### Java常用的原子类
AtomicBoolean、AtomicInteger 和 AtomicLong
