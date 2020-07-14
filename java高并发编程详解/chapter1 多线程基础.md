## 生命周期
![线程的生命周期](C:\program1\notes\java高并发编程详解\线程生命周期.png)

| 状态       | 含义           | 转变方法                                  |
| ---------- | -------------- | ----------------------------------------- |
| new        | Thread对象创建 | start()->转变为runnable状态(线程创建)     |
| runnable   | 可运行状态     | cpu时间片调度->转变为running; 意外终止    |
| running    | 运行中         | stop()或逻辑标识->terminated              |
|            |                | sleep();wait()加入waitSet;->blocked       |
|            |                | 阻塞IO操作,例如网络IO->bolcked            |
|            |                | 获取锁资源,加入该锁的阻塞队列->blocked    |
|            |                | cpu调度使线程放弃执行->runnable           |
|            |                | yiled();放弃cpu执行权力->runnable         |
|            | 阻塞中         | stop();意外死亡(JVM crash)->terminated    |
|            |                | 阻塞结束;sleep结束;->runnable             |
|            |                | 其他线程的notify();notifyAll();->runnable |
|            |                | 获取到了锁;->runnable                     |
|            |                | 阻塞被打断;interrupt->runnable            |
| terminated | 终结           | 正常结束;运行出错结束;->terminated        |
|            |                | JVM Crash 导致所有进程结束                |

## Thread的构造函数



## Thread的API

## 线程安全与数据同步

## 线程间通信

## ThreadGroup

## Hook线程以及捕获线程执行异常

## 线程池原理及自定义线程池