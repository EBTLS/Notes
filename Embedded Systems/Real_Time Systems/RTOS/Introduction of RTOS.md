# Introduction of RTOS

[toc]

> From: [https://zhuanlan.zhihu.com/p/86861756](https://zhuanlan.zhihu.com/p/86861756)
>

# 1. Definition

## 1.1. Real-Time Computing

***Real-time computing (RTC)*** , or reactive computing is the computer science term for hardware and software systems subject to a "real-time constraint", for example from event to system response.**Real-time programs must guarantee response within specified time constraints,** often referred to as "deadlines".

## 1.2.  Real-Time Operating Systems

A ***real-time operating system (RTOS)*** is an **operating system (OS)** intended to **serve real-time applications** that process data as it comes in, typically without buffer delays. Processing time requirements (including any OS delay) are measured in tenths of seconds or shorter increments of time. A real-time system is a **time-bound system** which has well-defined, fixed **time constraints** . **Processing must be done within the defined constraints** or the system will fail. They either are **event-driven** or **time-sharing** . Event-driven systems switch between tasks based on their priorities, while time-sharing systems switch the task based on clock interrupts. Most RTOSs use a pre-emptive scheduling algorithm.

## 1.3. Interrupt Latency & Interruption Handeler

In computing,***interrupt latency*** is the time that elapses from when an interrupt is **generated** to when the source of the interrupt is **serviced** .

* For many operating systems, devices are serviced as soon as the device's***interrupt handler*** is executed.
* Interrupt latency may be affected by microprocessor design, interrupt controllers, interrupt masking, and the operating system's (OS) interrupt handling methods.

In computer systems programming, an ***interrupt handler*** , also known as an ***interrupt service routine or ISR*** , is a special block of code associated with a specific interrupt condition.

* Interrupt handlers are initiated by hardware interrupts, software interrupt instructions, or software exceptions, and are used for implementing device drivers or transitions between protected modes of operation, such as system calls.

## 1.4. Context Switch

In computing, a ***context switch*** is the process of **storing the state of a process or thread,** so that it can be restored and resume execution at a later point.

* This**allows multiple processes to share a single central processing unit (CPU)** , and is an essential feature of a multitasking operating system.

For process or thread , see ((20210309170409-fftvciw "{{.text}}"))

# 2. Introduction of RTOS

## 2.1. Characteristics

* A**key characteristic** of an RTOS is**the level of its consistency concerning the amount of time it takes to accept and complete an application's [task](https://en.wikipedia.org/wiki/Task_(computing))** ; the variability is **jitter**.

  * A *'**hard' real-time operating system (Hard RTOS)*** has less jitter than a ***'soft' real-time operating system (Soft RTOS)*** .
  * The late answer is a wrong answer in a hard RTOS while a late answer is acceptable in a soft RTOS.
* The chief design goal **is not high throughput**, but rather a**guarantee** of a [soft or hard](https://en.wikipedia.org/wiki/Real-time_computing#Criteria_for_real-time_computing) performance category.
* A RTOS has an**advanced algorithm** for **scheduling**Scheduler flexibility enables a wider, computer-system orchestration of process priorities, but a real-time OS is more frequently dedicated to a narrow set of applications.
* Key factors in a real-time OS are**minimal [interrupt latency](https://en.wikipedia.org/wiki/Interrupt_latency)** and**minimal [thread switching latency](https://en.wikipedia.org/wiki/Thread_switching_latency)** ; a real-time OS is**valued more for how quickly or how predictably it can respond** than for the amount of work it can perform in a given period of time.[[3]](https://en.wikipedia.org/wiki/Real-time_operating_system#cite_note-3)

## 2.2. Classification

### Soft RTO

An RTOS that can **usually or generally** meet a deadline is a ***soft real-time OS** ,*

### Hard RTOS

but if it can meet a deadline **[deterministically](https://en.wikipedia.org/wiki/Deterministic_algorithm)** it is a ***hard real-time OS.***

# 3. Design of RTOS

## 3.1. 任务调度器 （Task Scheduler）

实时操作系统中**都要包含一个实时任务调度器** ，这个任务调度器与其它操作系统的最大不同是强调：严格按照优先级来分配CPU时间，并且时间片轮转不是实时调度器的一个必选项。

## 3.2. 实时的消息、事件处理机制。

常规的操作系统中，消息队列都是按照FIFO（先进先出）的方式进行调度，如果有多个接受者，那么接受者也是按照FIFO的原则接受消息（数据），但**实时操作系统会提供基于优先级的处理方式：**两个任务优先级是分别是10和20，同时等待一个信号量，如果按照优先级方式处理，则优先级为10的任务会优先收到信号量。

## 3.2. 提供内核级的优先级翻转处理方式

实时操作系统调度器最经常遇到的问题就是优先级翻转，因此对于类似信号量一类的API，都能**提供抑止优先级翻转的机制，防止操作系统死锁** 。

## 3.3. 减少粗粒度的锁和长期关中断的使用

这里的**锁** 主要是指***自旋锁(spinlock)***一类会影响中断的锁，也包括任何关中断的操作。

* 在Windows和Linux的驱动中，为了同步的需要，可能会长期关闭中断，这里的长期可能是毫秒到百微秒级。但实时操作系统通常不允许长期关中断
* 对于**非实时操作系统** 来说，如果收到一个外部中断，那么操作系统在处理中断的整个过程中可能会一直**关中断** 。
* 但实时操作系统的通常做法是把中断作为一个事件**通告给另外一个任务** ，interrupt handler在处理完关键数据以后，立即打开中断，驱动的中断处理程序以一个高优先级任务的方式继续执行。

## 3.4. 系统级的服务也要保证实时性

* 对于一些系统级的服务，比如文件系统操作，**非实时系统会缓存用户请求，并不直接把数据写入设备**，或者建立一系列的线程池，分发文件系统请求。但**实时系统中允许高优先级的任务优先写入数据** ，在文件系统提供服务的整个过程中，高优先级的请求被优先处理，这种高优先级策略直到操作完成。
* 这种设计实际上会**牺牲性能** ，但**实时系统强调的是整个系统层面的实时性** ，而不是某一个点（比如内核）的实时性，所以系统服务也要实时。
* 由于应用场景的差异，会出现有些用户需要实时性的驱动，有些用户需要高性能的驱动，因此实时操作系统实际上要提供**多种形式的配置** 以满足不同实时性需求的用户

## 3.5. 避免提供实时性不确定的API

* 多数实时操作系统都**不支持*虚拟内存（page file/swap area）*** ，主要原因是***缺页中断*（page fault）会导致任务调度的不确定性增加** 。
* 实时操作系统**很多都支持分页，**但很少会使用虚拟内存，因为一次** 缺页中断的开销十分巨大** （通常都是毫秒级），波及的代码很多，导致用户程序执行的不确定性增加。
* 实时操作系统的**确定性** 是一个很重要的指标，在某些极端场景下，甚至会禁用动态内存分配（malloc/free），来保证系统不受到动态的任务变化的干扰。

## 3.6. 提供针对实时系统调度的专用API

比如ARINC 653标准中就针对任务调度等作出了一系列的规定，同时定义了特定的API接口和API行为，这些API不同于POSIX API，如果实时系统要在航空设备上使用，就可能需要满足ARINC 653的规范。

## 3.7. 降低系统抖动

由于关中断等原因，通常情况下，操作系统的**调度器不会太精确的产生周期性的调度** ，比如x86早期的默认60的时钟周期（clock rate），抖动范围可能在15-17ms之间。但一个设计优秀的实时操作系统能把**调度器的抖动降低到微秒甚至百纳秒一级** ，在像x86这种天生抖动就很大的架构上，降低系统抖动尤其重要。

## 3.8. 针对实时性设计的SMP和虚拟化技术

***SMP（多核)***场景的实时调度是很困难的，这里还涉及到任务核间迁移的开销。针对SMP场景，多数实时操作系统的设计都不算十分优秀，但比起普通操作系统来说，其实时性已经好很多了。

同时实时操作系统的虚拟化能从hypervisor层面上提供虚拟机级别的实时调度，虚拟机上可以是另外一个实时系统，也可以是一个非实时系统