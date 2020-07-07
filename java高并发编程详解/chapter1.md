#### 生命周期
![线程的生命周期](C:\program1\notes\java高并发编程详解\线程生命周期.png)

**new:** thread创建之后(new,此时还是一个普通对象),通过start(); 进入runnable状态(转变为一个jvm中存在的线程)

**runnable:** 等待cpu调度执行,只能转变为running或者直接意外终止,因为转变其他状态都需要cpu调度

**