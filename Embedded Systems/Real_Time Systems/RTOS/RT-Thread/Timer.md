# Timer

[toc]

RT-Thread does not provide ways to directly define a periodic task. But a periodic task can be created by using clock \& timer management of RT-Thread.

The smallest time unit in the operating system is clock tick (OS Tick). A clock tick is a periodically triggered interrupt. When a timer runs out of its time (reach its timeout), a timeout function will be executed. There are two modes of the timer in RT-Thread,``HARD_TIMER Mode`` and ``SOFT_TIMER Mode``. In ``HARD_TIMER Mode``, the timeout function executes the interrupt context, which means it should be finished as soon as possible. In ``SOFT_TIMER Mode``, a timer thread is created, and the timeout function execute the timer thread context, which can do some complex and long-time operations.

RT-Thread has a self-add global variable ``rt_tick`` and maintains an ordered list of timers. ``rt_tick_increase`` increase one each OS Tick. The ordered list is ordered by the timeout tick set. Each OS Tick, the system will check the head of the ordered list whether it is equal to the ``rt_tick``, a sporadic timer triggered will be removed, and a periodic timer triggered will change its timeout setting and put in the queue again.

RT-Thread provides 4 kinds of operations to a timer: create/initiate, start, stop/control, delete/detach.

```cpp
rt_system_timer_init() /**< initialize timer management system*/
rt_system_timer_thread_init()  /**< initialize SOFT_TIMER*/
rt_timer_create()  /**< dynamically creating a timer*/
rt_timer_delete()  /**< delete a dynamic timer (detach and release the memory)*/
rt_timer_init()  /**< statically create a timer*/
rt_timer_detach()  /**< delete a static timer (detach from the queue)*/
rt_timer_start  /**< activate a timer (put the timer into the ordered linked list)*/
rt_timer_stop  /**< stop a timer (detach from the queue)*/
rt_timer_control  /**< change timer configuration*/
```