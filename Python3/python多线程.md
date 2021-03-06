## 多线程编程

多线程任务特点：

本质上就是异步的，需要有多个并发事务，各个事务的运行顺序可以不确定、随机的。

运算密集型任务：容易分隔成多个子任务，



使用多线程编程和一个共享的数据结构如Queue（一种多线程队列数据结构）

- UserRequestThread: 负责读取客户的输入，可能是一个I/O信道，可创建多个线程，每个客户一个，请求会被放入队列中。
- RequestProcessor: 负责从队列中获取并处理请求的线程，为下面的线程提供输出。
- ReplyThread: 负责把给用户的输出取出来，如果是网络应用程序，就把结果发出去，否则就保存到本地系统或数据库中。



#### 进程：

是程序的一次执行，有自己的地址空间、内存、数据栈，进程间职能使用IPC进行通信

### 线程

所有线程运行在同一进程中，共享相同的运行环境。

线程有开始，顺序执行和结束三部分，

### GIL 全局解释锁

在任意时刻，只有一个线程在解释器中运行。对Python 虚拟机的访问由全局解释器锁（GIL）来控制，GIL保证了同一时刻只有一个线程在运行。

```
1、设置GIL
2、切换到一个线程去运行
3、运行：
	a. 指定数量的字节码的指令，或者
	b. 线程主动让出控制（调用time.sleep(0)）
4、把线程设置为睡眠状态。
5、解锁GIL
6、再次重复以上所有步骤。
```



#### Python 的 threading 模块

多线程编程模块：包括thread、threading 和 Queue等。

thread、threading模块允许程序员创建和管理线程。

Queue 模块允许用户创建一个可以用于多个线程之间共享数据的队列数据结构。

- **_thread/thread模块**
  提供了基本的同步数据结构锁对象（lock object, 也叫原语锁、简单锁、互斥锁、互斥量、二值信号量）
  | 函数                                          | 描述                                                         |
  | --------------------------------------------- | ------------------------------------------------------------ |
  | 模块函数                                      |                                                              |
  | start_new_thread(function, args, kwarts=None) | 产生一个新的线程，在新的线程中用指定的参数和可选的kwargs来调用这个函数。参数为：参数，函数参数以及可选的关键字参数。 |
  | allocate_lock()                               | 分配一个LockType类型的锁对象                                 |
  | exit()                                        | 让线程退出                                                   |
  | LockType类型锁对象方法                        |                                                              |
  | acquire(wait=None)                            | 尝试获取锁对象                                               |
  | locked()                                      | 如果获取了锁对象返回True，否则返回False                      |
  | release()                                     | 释放锁
                                                         |

- **threading模块**
  支持守护线程，

  | threading  模块对象          | 描述                                                         |
  | -------------------------- | ------------------------------------------------------------ |
  | Thread                     | 表示一个线程的执行对象                                        |
  | Lock                       | 锁原语对象(跟thread模块里的锁对象相同)                         |
  | RLock                      | 可重入锁对象，使单线程可以再次获取已经获得了的锁（递归锁定）        |
  | Condition                  | 条件变量能让一个线程停下来，等待其他线程满足了某个“条件”，如状态改变或值的改变 |
  | Event                      | 通用的条件变量。多个线程可以等待某个事情的发生，在事件发生后，所有线程都被激活 |
  | Semaphore                  | 为等待锁的线程提供一个类型“等候室”的结构。
  | BoundedSemaphore           | 与Semaphore 类似，只是它不允许超过初始值                       |
  | Timer                      | 与Thread相似，只要它等待一段时间后才开始运行                     |

 
  | threading  模块对象          | 描述                                                         |
  | -------------------------- | ------------------------------------------------------------ |
  | threading.currentThread(): | 返回当前的线程变量                                           |
  | threading.enumerate():     | 返回一个包含正在运行的线程的list。正在运行指线程启动后、结束前，不包括启动前和终止后的线程。 |
  | threading.activeCount():   | 返回正在运行的线程数量，与len(threading.enumerate())有相同的结果。 |

  | threading 模块下 Thread 类方法 | 描述                                                       |
  | -------------------------- | ------------------------------------------------------------ |
  |                            |                                                              |
  | **start():**               | 启动线程活动。                                                 |
  | **run():**                 | 用以表示线程活动的方法。                                         |
  | **join(timeout=None):**          | 等待至线程中止。如果给了timeout, 则最多阻塞timeout秒         |
  | **isAlive():**             | 返回线程是否活动的。                                         |
  | **getName():**             | 返回线程名                                                   |
  | **setName():**             | 设置线程名                                                   |
  | **isDaemon():**            | 返回线程的 daemon 标志                                       |
  | **setDaemon(darmonic)**.   | 把线程的daemon 标志设为daemonic(一定要在调用start()函数前调用)



  ​