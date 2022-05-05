# Some Preparations

[toc]



# 1. 自旋锁

是指当一个线程在获取锁的时候，如果锁已经被其它线程获取，那么该线程将循环等待，然后不断的判断锁是否能够被成功获取，直到获取到锁才会退出循环。 

获取锁的线程一直处于活跃状态，但是并没有执行任何有效的任务，使用这种锁会造成**busy-waiting**。

它是为实现保护共享资源而提出一种锁机制。其实，自旋锁与互斥锁比较类似，它们都是为了解决对某项资源的互斥使用。无论是互斥锁，还是自旋锁，在任何时刻，最多只能有一个保持者，也就说，在任何时刻最多只能有一个执行单元获得锁。但是两者在调度机制上略有不同。

* 对于互斥锁，如果资源已经被占用，**资源申请者只能进入睡眠状态**。
* 自旋锁不会引起调用者睡眠，如果自旋锁已经被别的执行单元保持，调用者就**一直循环在那里**看是否该自旋锁的保持者已经释放了锁，"自旋"一词就是因此而得名。



# 2. 引用和指针

## 2.3. 引用VS 指针

1. 引用必须被初始化，指针不必。
2. 引用初始化以后不能被改变，指针可以改变所指的对象。
3. 不存在指向空值的引用，但是存在指向空值的指针。





# 3. Static



## static的作用





# 4. 死锁

## 4.1. 产生死锁的原因

多个并发进程因争夺系统资源而产生相互等待的现象。即：一组进程中的每个进程都在等待某个事件发生，而只有这组进程中的其他进程才能触发该事件，这就称这组进程发生了死锁。

产生死锁的本质原因为：

1. 系统资源有限。
2. 进程推进顺序不合理



## 4.2. 死锁的四个必要条件

1. 互斥：某种资源一次只允许一个进程访问，即该资源一旦分配给某个进程，其他进程就不能再访问，直到该进程访问结束。
2. 占有且等待：一个进程本身占有资源（一种或多种），同时还有资源未得到满足，正在等待其他进程释放该资源。
3. 不可抢占：别人已经占有了某项资源，你不能因为自己也需要该资源，就去把别人的资源抢过来。
4. 循环等待：存在一个进程链，使得每个进程都占有下一个进程所需的至少一种资源。



## 4.3. 死锁的处理方式：

**<u>预防死锁</u>**：

1. 资源一次性分配：（破坏请求和保持条件）
2. 可剥夺资源：即当某进程新的资源未满足时，释放已占有的资源（破坏不可剥夺条件）
3. 资源有序分配法：系统给每类资源赋予一个编号，每一个进程按编号递增的顺序请求资源，释放则相反（破坏环路等待条件）

**避免死锁**:

预防死锁的几种策略，会**严重地损害系统性能**。因此在避免死锁时，要施加较弱的限制，从而获得 较满意的系统性能。由于在避免死锁的策略中，允许进程动态地申请资源。因而，系统在进行资源分配之前预先计算资源分配的安全性。若此次分配不会导致系统进入不安全状态，则将资源分配给进程；否则，进程等待。其中最具有代表性的避免死锁算法是银行家算法。

**检测死锁**：

首先为每个进程和每个资源指定一个唯一的号码；

然后建立资源分配表和进程等待表

**解除死锁**：

当发现有进程死锁后，便应立即把它从死锁状态中解脱出来，常采用的方法有：

1. 剥夺资源：从其它进程剥夺足够数量的资源给死锁进程，以解除死锁状态；
2. 撤消进程：可以直接撤消死锁进程或撤消代价最小的进程，直至有足够的资源可用，死锁状态.消除为止；所谓代价是指优先级、运行代价、进程的重要性和价值等。



## 4.4. 进程与线程

1. 进程是**资源分配的最小单位**。
2. 线程是**程序执行的最小单位**，也是处理器调度的基本单位，但进程不是，两者均可并发执行。
3. 进程有自己的**独立地址空间**，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据，使用相同的地址空间，因此，CPU切换一个线程的花费远比进程小很多，同时创建一个线程的开销也比进程小很多。
4. **线程之间的通信更方便**，同一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式（IPC)进行。不过如何处理好同步与互斥是编写多线程程序的难点。但是**多进程程序更健壮**，多线程程序只要有一个线程死掉，整个进程也跟着死掉了，而一个进程死掉并不会对另外一个进程造成影响，因为进程有自己独立的地址空间。
5. 进程切换时，消耗的资源大，效率低。所以涉及到频繁的切换时，使用线程要好于进程。同样如果要求同时进行并且又要共享某些变量的并发操作，只能用线程不能用进程。
6. 执行过程：每个独立的进程有一个程序运行的入口、顺序执行序列和程序入口。但是线程不能独立执行，必须依存在应用程序中，由应用程序提供多个线程执行控制。

优缺点：

线程执行开销小，但是不利于资源的管理和保护。线程适合在SMP机器（双CPU系统）上运行。

进程执行开销大，但是能够很好的进行资源管理和保护，可以跨机器迁移。

何时使用多进程，何时使用多线程？

对资源的管理和保护要求高，不限制开销和效率时，使用多进程。

要求效率高，频繁切换时，资源的保护管理要求不是很高时，使用多线程。