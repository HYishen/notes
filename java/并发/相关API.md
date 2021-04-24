### Object
```
java.lang.Object#wait()

java.lang.Object#notify

java.lang.Object#notifyAll
```

### java.lang.Thread
```
// t.join()方法阻塞调用此方法的线程(calling thread)，直到线程t完成，此线程再继续
java.lang.Thread#join()
```

### java.util.concurrent.locks.Lock
```
java.util.concurrent.locks.Lock#lock

java.util.concurrent.locks.Lock#tryLock()

java.util.concurrent.locks.Lock#unlock

java.util.concurrent.locks.Lock#newCondition
```

### java.util.concurrent.locks.Condition
```
java.util.concurrent.locks.Condition#await()

java.util.concurrent.locks.Condition#signal

java.util.concurrent.locks.Condition#signalAll
```
