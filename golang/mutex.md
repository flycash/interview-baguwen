# Mutex 

## 面试题

### `mutex`是如何加锁的

分析：也是考察`mutex`的实现原理。基本上就是围绕`自旋-FIFO`来说的。简单理解就是，`mutex`先尝试自旋，自旋不行就所有`goroutine`步入`FIFO`，先到先得。

加锁的这个步骤，讲得非常详细。

答案：`mutex`加锁大概分成两种模式：
1. 在正常模式下，`goroutine`通过自旋来获得锁；
2. 但是如果存在一个`goroutine`等待锁超过了`1ms`，那么`mutex`就会进入饥饿模式，在饥饿模式下，会遵循`FIFO`原则，将锁交给下一个`goroutine`。也就是从等待队列里面唤醒第一个等待者；

（讨论一下公平性的问题）所以从严格意义上来说，它并不是公平锁，因为在正常状态下，一个新的请求锁的`goroutine`和等待的`goroutine`一起竞争锁。而严格意义的公平应该是永远遵循 `FIFO`。


#### 类似问题
- 什么是饥饿状态
- 是不是公平锁


## Reference
[mutex is more fair](https://news.ycombinator.com/item?id=15096463)
[mutex fairness]()
[这可能是最容易理解的 Go Mutex 源码剖析](https://segmentfault.com/a/1190000039855697)
