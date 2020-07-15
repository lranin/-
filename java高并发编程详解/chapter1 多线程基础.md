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

``` java
public synchronized void start() {
    /**
     * This method is not invoked for the main method thread or "system"
     * group threads created/set up by the VM. Any new functionality added
     * to this method in the future may have to also be added to the VM.
     *
     * A zero status value corresponds to state "NEW".
     */
    //status is "NEW"?
    if (threadStatus != 0)
        throw new IllegalThreadStateException();

    /* Notify the group that this thread is about to be started
     * so that it can be added to the group's list of threads
     * and the group's unstarted count can be decremented. */
    group.add(this);

    boolean started = false;
    try {
        //jndi方法,在其中调用了线程的run()
        start0();
        //疑问:jvm保证了指令排序?
        started = true;
    } finally {
        try {
            if (!started) {
                group.threadStartFailed(this);
            }
        } catch (Throwable ignore) {
            /* do nothing. If start0 threw a Throwable then
              it will be passed up the call stack */
        }
    }
}
```



```java
private void init(ThreadGroup g, Runnable target, String name,
                  long stackSize, AccessControlContext acc,
                  boolean inheritThreadLocals) {
    if (name == null) {
        throw new NullPointerException("name cannot be null");
    }

    this.name = name;
    
	//父线程为创建该线程的线程
    Thread parent = currentThread();
    SecurityManager security = System.getSecurityManager();
    if (g == null) {
        /* Determine if it's an applet or not */

        /* If there is a security manager, ask the security manager
           what to do. */
        if (security != null) {
            //如果有安全管理器方案,那么按照安全管理器的方案进行初始化父线程组
            g = security.getThreadGroup();
        }

        /* If the security doesn't have a strong opinion of the matter
           use the parent thread group. */
        if (g == null) {
            //默认加入父线程的线程组
            g = parent.getThreadGroup();
        }
    }

    /* checkAccess regardless of whether or not threadgroup is
       explicitly passed in. */
    g.checkAccess();

    /*
     * Do we have the required permissions?
     */
    if (security != null) {
        if (isCCLOverridden(getClass())) {
            security.checkPermission(SUBCLASS_IMPLEMENTATION_PERMISSION);
        }
    }
```



* 什么是守护线程

* 为什么要有守护线程

* 什么时候需要守护线程

  ※The Java Virtual Machine exits when the only threads running are all daemon threads.

1. 守护线程的属性deamon is true
2. 守护线程是具有传递性的
3. 守护线程的特性是它的生命周期是会自动结束的,在没有非守护线程存活时,它会自动terminated
4. 守护线程通常作为后台线程存在,例如游戏和服务器通信的线程

## Thread的API

### sleep()

1. sleep方法会使当前线程进入指定毫秒数的休眠，暂停执行(blocked)
2. 虽然给定了一个休眠的时间，但是最终要以系统的定时器和调度器的精度为准
3. 休眠有一个非常重要的特性，那就是其不会放弃monitor锁的所有权
4. 只会让当前线程进入休眠
5. 建议使用TimUtil替代Thread.sleep();

### yield()和priority()

* 都是不可靠方法,即无法保证属性在所有情况下都一定会生效
* yield()目标是告知CPU当前线程会放弃执行机会
* 优先级高的线程"可能"会获得更多的CPU执行机会

### getId()

* 获取指定线程的线程ID

### currentThread()

* 获取当前线程

### setContextClassLoader()

* 设置当前线程的类加载器

### interrput()

* Object 的 wait 方法; wait(long); wait(long, int)方法。

* Thread 的 sleep(long)方法。 Thread 的 sleep（ long， int）方法。 
* Thread 的 join 方法。 Thread 的 join（ long）方法。 Thread 的 join（ long， int）方法。 
* InterruptibleChannel 的 io 操作。 
* Selector 的 wakeup 方法。 
* 其他方法

上述方法都会将线程进入blocked状态

调用interrupt(),会将线程状态从blocked状态重置为runnable

### join()

```java
public final synchronized void join(long millis)
throws InterruptedException {
    long base = System.currentTimeMillis();
    long now = 0;

    if (millis < 0) {
        throw new IllegalArgumentException("timeout value is negative");
    }

    if (millis == 0) {
        while (isAlive()) {
            wait(0);
        }
    } else {
        while (isAlive()) {
            long delay = millis - now;
            if (delay <= 0) {
                break;
            }
            wait(delay);
            now = System.currentTimeMillis() - base;
        }
    }
}
```



## 线程安全与数据同步

## 线程间通信

## ThreadGroup

## Hook线程以及捕获线程执行异常

## 线程池原理及自定义线程池