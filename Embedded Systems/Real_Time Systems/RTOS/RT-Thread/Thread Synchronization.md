# Thread Synchronization

[toc]

## 1.1. Semaphore

Semaphore is a light-duty kernel object that can solve the problems of synchronization between threads.

* By **obtaining or releasing semaphore**, a thread can achieve synchronization or mutual exclusion.
* Semaphore value corresponds to the actual number of instances of semaphore object and the number of resources.

### Details of Semaphore In RT-Thread

* Thread **obtains semaphore** resource instances by obtaining semaphores.

  * When the semaphore value is **greater than zero, the thread will get the semaphore**, and the corresponding semaphore value will **be reduced by 1**
  * if the value of the semaphore **is zero**, it means the current semaphore resource instance **is not available**, and the thread applying for the semaphore will choose according to the time parameters to either **return directly**, or **suspend**for a period of time, or wait forever.
  * While waiting, if other threads or ISR **released the semaphore**, then the thread will **stop the waiting**.
* Semaphore is released to **wake up the thread that suspends** on the semaphore

  * when semaphore value is zero and a thread is waiting for this semaphore, releasing the semaphore will**wake up the first thread waiting in the thread queue** of the semaphore, and this thread will obtain the semaphore;
  * otherwise value of the semaphore **will plus 1.**


```cpp
struct rt_semaphore
{
    struct rt_ipc_object parent;            /**< inherit from ipc_object */
    rt_uint16_t          value;             /**< value of semaphore. */
    rt_uint16_t          reserved;          /**< reserved field */
};
typedef struct rt_semaphore *rt_sem_t;

rt_sem_t dynamic_sem = RT_NULL;             //Define semaphore memory target.
rt_sem_create("dsem", 0, RT_IPC_FLAG_FIFO); //Create the semaphore with FIFO.
rt_sem_delete(dynamic_sem);                 //Delete the semaphore target.
rt_sem_take(dynamic_sem, RT_WAITING_FOREVER); //Try to take a semaphore, wait forever if fails. The second parameter can be a time which indicates the upper bound of waiting time (Unit: system tick). Return RT_ETIMEOUT after the waiting time.
rt_sem_trytake(dynamic_sem);                //Try to take a semaphore without waiting. Return RT_ETIMEOUT immediately if fail.
rt_sem_release(dynamic_sem);                //Release a semaphore, wake up other threads waiting for this semaphore.
```

### Usage of Semaphore

#### Thread Synchronization

This occasion can also be seen as using the semaphore for the work completion flag: the thread holding the semaphore completes its own work, and **then notifies the thread waiting for the semaphore to continue the next part of the work.**

#### Lock

Locks, a single lock is often applied to multiple threads accessing the same shared resource (in other words, critical region). When semaphore is used as a lock, the semaphore resource instance **should normally be initialized to 1**, indicating that the
system has one resource available by default

#### Interrupt Synchronization between Threads

#### Resource Count

## 1.2. Mutex

Mutexes, also known as **mutually exclusive semaphores**, are a special **binary semaphore**.

* The **difference**between a mutex and a semaphore is that the thread with a mutex has **ownership of the mutex**, mutex supports recursive access and prevents thread priority from reversing; and mutex can only be released by the thread holding it, whereas
  semaphore can be released by any thread.
* There are only two states of mutex, **unlocked** and **locked** (two state values).

```cpp
struct rt_mutex
{
    struct rt_ipc_object parent;            /**< inherit from ipc_object */
    rt_uint16_t          value;             /**< value of mutex */
    rt_uint8_t           original_priority; /**< priority of last thread hold the mutex */
    rt_uint8_t           hold;              /**< numbers of thread hold the mutex */
    struct rt_thread    *owner;             /**< current owner of mutex */
};
typedef struct rt_mutex *rt_mutex_t;

rt_mutex_t dynamic_mutex = RT_NULL;         //Define a mutex memory space
dynamic_mutex = rt_mutex_create("dmutex", RT_IPC_FLAG_FIFO);    //Initialize the mutex memory
rt_err_t rt_mutex_delete (rt_mutex_t mutex);//Delete the mutex
rt_mutex_take(dynamic_mutex, RT_WAITING_FOREVER); //Try to take a semaphore, wait forever if fails. The second parameter can be a time which indicates the upper bound of waiting time (Unit: system tick). Return RT_ETIMEOUT after the waiting time.
rt_err_t rt_mutex_release(rt_mutex_t mutex);//Release (unlock) a mutex, allow other threads to lock this mutex

```

## 1.3. Event Set

Event set is also one of the mechanisms for synchronization between threads. An event set can contain multiple events. Event set can be used to complete **one-to-many, many-
to-many thread synchronization**

* Unlike mutex and semaphore, event can be used to handle large range of signals and apply **basic logic operations**on them to decide execute it or not. For example, thread 1 can only resume when allthree events have arrived


### Details of Event Set in RT-Thread

![image.png](assets/image-20210319233918-vn22z6p.png)

RT-thread uses a uint32_t to represent the event flags. Because there are 32 bits for uint32_t,RT-thread **support at most receiving 32 event flags at the same time for a single thread.** In addition,for multi threads blocked by a single event, the next task to resume is determined by the **FIFO or priority-based strategy**.

```cpp
struct rt_event
{
    struct rt_ipc_object parent;    /**< inherit from ipc_object */
    rt_uint32_t          set;       /**< event set */
};
typedef struct rt_event *rt_event_t;

struct rt_event event;                          //Define a mutex memory space
rt_event_init(&event, "event", RT_IPC_FLAG_FIFO);   //Initialize the event memory
rt_event_send(&event, EVENT_FLAG3);             //Send an event with event flag3
rt_event_recv(&event, (EVENT_FLAG3 | EVENT_FLAG5), RT_EVENT_FLAG_AND | RT_EVENT_FLAG_CLEAR, RT_WAITING_FOREVER, &e); //Wait until receive the event with both event flag 3 and flag 5 are set. The RT_EVENT_FLAG_CLEAR means automatically clear the flag when this function returns. The last parameter stores the current event value
```