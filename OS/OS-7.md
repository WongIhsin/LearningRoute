# 网易公开课——清华大学公开课：操作系统

---

+ 背景
+ 信号量
+ 信号量使用
+ 信号量实现
+ 管程
+ 经典同步问题

---

## 10.1背景

+ 并发问题：竞争条件（竞态条件）
  + 多程序并发存在大的问题
+ 同步
  + 多线程共享公共数据的协调执行
  + 包括互斥与条件同步
  + 互斥：在同一时间只有一个线程可以执行临界区
+ 确保同步正确很难？
  + 需要高层次的编程抽象（如：锁）
  + 从底层硬件支持编程

---

## 10.2信号量

仅仅保证互斥，一些问题还是无法解决

+ 抽象数据类型
  + 一个整形（sem），两个原子操作
  + P()：sem减1，如果sem<0，等待，否则继续
  + V()：sem加1，如果sem<=0，唤醒一个等待的P
+ 信号量类似铁路
  + 初始化两个资源控制信号灯
+ Dijkstra在20世纪60年代提出
  + V：Verhoog（荷兰语增加)
  + P：Prolaag（荷兰语简称“Probeer te Verlagen”，或尝试减少）
+ 在早期的操作系统是主要的同步原语
  + 例如，原Unix
  + 现在很少用（但还是在计算机科学研究非常重要）

---

## 10.3信号量的使用

+ 信号量是==整数==
+ 信号量是==被保护==的变量
  + 初始化完成后，唯一改变一个信号量的值的办法是通过P()和V()
  + 操作必须是原子
+ ==P()能够阻塞==，V()不会阻塞
+ 我们假定信号量是“公平的”
  + 没有线程被阻塞在P()仍然堵塞如果V()被无限频繁调用（在同一个信号量）
  + 在实践中，FIFO经常被使用

锁的忙等待没有FIFO这样的公平性

---

+ 两种类型信号量
  + ==二进制信号量==：可以是0或1
  + ==一般/计数信号量==：可取任何非负值
  + 两者相互表现（给定一个可以实现另一个）
+ 信号量可以用在两个方面
  + 互斥
  + 条件同步（调度约束——一个线程等待另一个线程的事情发生）

---

+ 用二进制信号量实现的互斥

  + ```c
    mutex = new Semaphore(1);
    mutex->P();
    ...
    Critical Section;
    ...
    mutex->V();
    ```

+ 用二进制信号量实现的调度约束

  + ```c
    condition = new Semaphore(0);
    //Thread A
    ...
    condition->P();
    ...
    //Thread B
    ...
    condition->V();
    ...
    ```

  + P()等待，V()发出信号

---

+ 一个线程等待另一个线程处理事情
  + 比如生成东西或消费东西
  + 互斥（锁机制）是不够的

+ 例如：有界缓冲区的生产者——消费者问题
  + 一个或多个==生产者==产生数据将数据放在一个缓冲区里
  + 单个==消费者==每次从缓冲区取出数据
  + 在任何一个时间==只有一个==生产者或消费者可以访问该缓冲区
+ 正确性要求
  + 在任何一个时间只能由一个线程操作缓冲区（互斥）
  + 当缓冲区为空，消费者必须等待生产者（调度/同步约束）
  + 当缓冲区满，生产者必须等待消费者（调度/同步约束）
+ 每个约束用一个单独的信号量
  + 二进制信号量互斥
  + 一般信号量fullBuffers
  + 一般信号量emptyBuffers

```c
Class BoundedBuffer {
    mutex = new Semaphore(1);
    fullBuffers = new Semaphore(0);
    emptyBuffers = new Semaphore(n);
}
////生产者
BoundedBuffer::Deposit(c) {
    emptyBuffers->P();
    mutex->P();
    Add c to the buffer;
    mutex->V();
    fullBuffers->V();
}
////消费者
BoundedBuffer::Remove(c) {
    fullBuffers->P();
    mutex->P();
    Remove c from buffer;
    mutex->V();
    emptyBuffers->V();
}
```

#### V()操作交换顺序没有关系，P()操作不能交换顺序，会死锁

P产生阻塞，容易产生死锁；V唤醒，可以交换顺序

---

## 10.4信号量的实现

使用硬件原语

+ 禁用中断
+ 原子指令（test-and-set）

类似锁

例如：使用"禁用中断"

```c
class Semaphore {
    int sem;
    WaitQueue q;
}

Semaphore::P() {
    sem--;
    if (sem < 0) {
        Add this thread t to q;
        block(p);
    }
}

Semaphore::V() {
    sem++;
    if (sem <= 0) {
        Remove a thread t from q;
        wakeup(t);
    }
}
```

+ 信号量的双用途
  + 互斥和条件同步
  + 但等待条件是独立的互斥

+ 读/开发代码比较困难
  + 程序员必须非常精通信号量
+ 容易出错
  + 使用的信号量已经被另一个线程占用
  + 忘记释放信号量
+ 不能够处理死锁问题

---

## 10.5管程Monitors

管程一开始用于解决编程语言的并发机制，而不是OS的同步互斥

+ 目的：分离互斥和条件同步的关注
+ 什么是管程
  + 一个锁：指定临界区，管程的临界区
  + 0或者多个条件变量：等待/通知信号量用于管理并发访问共享数据
+ 一般方法
  + 收集在对象/模块中的相关共享数据
  + 定义方法来访问共享数据

---

+ Lock
  + Lock::Acquire()——等待直到锁可用，然后抢占锁
  + Lock::Release()——释放锁，唤醒等待者如果有
+ Condition Variable条件变量
  + 允许等待状态进入临界区
    + 允许处于等待（睡眠）的线程进入临界区
    + 某个时刻原子释放锁进入睡眠
  + Wait() operation
    + 释放锁，睡眠，重新获得锁返回后
  + Signal() operation (or broadcast() operation)
    + 唤醒等待者（或者所有等待者），如果有

---

+ 条件变量的实现
  + 需要维持每个条件队列
  + 线程等待的条件等待signal()

```c
Class Condition {
    int numWaiting = 0;
    WaitQueue q;
}

Condition::Wait(lock) {
    numWaiting++;
    Add this thread t to q;
    release(lock);
    schedule(); // need mutex
    require(lock);
}

Condition::Signal() {
    if (numWaiting > 0) {
        Remove a thread t from q;
        wakeup(t); // need mutex
        numWaiting--;
    }
}
```

#### 生产者消费者问题的管程解决

```c
classBoundedBuffer {
    ...
    Lock lock;
    int count = 0;
    Condition notFull, notEmpty;
}
////生产者
BoundedBuffer::Deposit(c) {
    lock->Acquire();
    while (count == n)
        notFull.Wait(&lock);
    Add c to the buffer;
    count++;
    notEmpty.Signal();
    lock->Release();
}
////消费者
BoundedBuffer::Remove(c) {
    lock->Acquire();
    while (count == 0)
        notEmpty.Wait(&lock);
    Remove c from buffer;
    count--;
    notFull.Signal();
    lock->Release();
}
```

#### 两种Signal()方法

+ Hansen-style (most real OSes, or Java, Mesa)
  + 执行Signal，等到自己的lock在release()之后再切换
+ Hoare-style (most textbooks)
  + 一旦执行Signal就切换进程，最后切换回来才lock的release()，这时可使用if来代替while

---

+ Hansen-style
  + Signal is only a "hint" that the condition may be true
  + Need to check again
+ Benefits
  + Efficient implementation
+ Hoare-style
  + Cleaner, good for proofs
  + When a condition variable is signaled, it does not change
+ But
  + Inefficient implementation

---

+ 开发/调试并行程序很难
  + 非确定性的交叉指令
+ 同步结构
  + 锁：互斥
  + 条件变量：有条件的同步
  + 其他原语：信号量
+ 怎样有效的使用这些结构
  + 制定并遵循严格的程序设计风格/策略

---

## 10.6经典同步问题

+ #### 生产者-消费者问题

  + 见上文

+ #### 读者-写者问题

  + 动机
    + 共享数据的访问
  + 两种类型使用者
    + 读者：不需要修改数据
    + 写者：读取和修改数据
  + 问题的约束
    + 允许同一时间有多个读者，但在任何时候只有一个写者
    + 当没有写者时读者才能访问数据
    + 当没有读者和写者时，写者才能访问数据
    + 在任何时候只能有一个线程可以操作共享变量
  + 读者优先
    + 后来的读者可以跳过先来的写者先对数据进行访问
    + 基于读者优先策略的方法，只要有一个读者处于活动状态，后来的读者都会被接纳，如果读者源源不断地出现的话，那么写者就始终处于阻塞状态
  + 写者优先
    + 基于写者优先策略的方法：一旦写者就绪，那么写者会尽可能快地执行写操作，如果写者源源不断地出现的话，那么读者就始终处于阻塞状态

+ #### 哲学家就餐问题

  + 问题描述：（1965年由Dijkstra首先提出并解决）5个哲学家围绕一张圆桌而坐，桌子上放着5支叉子，每两个哲学家之间放一支；哲学家的动作包括思考和进餐，进餐时需要同时拿起他做左边和右边的两支叉子，思考时则同时将两支叉子放回原处。如何保证哲学家们的动作有序进行？如：不出现有人永远拿不到叉子



---

#### 读者-写者问题——信号量实现

+ 多个并发进程的数据集共享

  + 读者——只读数据集；它们不执行任何更新
  + 写者——可以读取和写入

+ 共享数据

  + 数据集
  + 信号量CountMutex初始化为1
  + 信号量WriteMutex初始化为1
  + 整数Rcount初始化为0

+ ```c
  //读者优先
  //---------------------------------------------
  //Writer
  sem_wait(WriteMutex); //相当于WriteMutex的P()操作
  
      write;
  
  sem_post(WriteMutex); //相当于WriteMutex的V()操作
  //---------------------------------------------
  //Reader
  sem_wait(CountMutex);
  if (Rcount == 0)      //多个读者可以直接读
      sem_wait(WriteMutex); //只有第一个读者会检查是不是有写者在写
  //如果有写者在写，则其他读者会阻塞在sem_wait(CountMutex)
  ++Rcount;
  sem_post(CountMutex); //对Rcount的互斥保护
  
  read;
  
  sem_wait(CountMutex);
  --Rcount;
  if (Rcount == 0)
      sem_post(WriteMutex);
  sem_post(CountMutex);
  //---------------------------------------------
  ```

---

#### 读者-写者问题——管程实现

+ 伪代码

  ```c
  Database::Read() {
      Wait until no writers;
      read database;
      check out - wake up waiting writers;
  }
  ```

  ```c
  Database::Write() {
      Wait util no readers/writers;
      write database;
      check out - wake up waiting readers/writers;
  }
  ```

+ 管程状态变量

  ```c
  AR = 0;        // # of active readers
  AW = 0;        // # of active writers
  WR = 0;        // # of waiting readers
  WW = 0;        // # of waiting writers
  Condition okToRead;
  Condition okToWrite;
  Lock lock;
  ```

+ ```c
  Public Database::Read() {
      //Wait until no writers;
      StartRead();
      read database;
      //check out - wake up waiting writers;
      DoneRead();
  }
  ```

  ```c
  Private Database::StartRead() {
      lock.Acquire();
      while ((AW+WW) > 0) { //体现了写者优先
          WR++;
          okToRead.wait(&lock);
          WR--;
      }
      AR++;
      lock.Release();
  }
  
  Private Database::DoneRead() {
      lock.Acquire();
      AR--;
      if (AR == 0 && WW > 0) {
          okToWrite.signal();
      }
      lock.Release();
  }
  ```

+ ```c
  Public Database::Write() {
      //Wait util no readers/writers;
      StartWrite();
      write database;
      //check out - wake up waiting readers/writers;
      DoneWrite();
  }
  ```

  ```c
  Private Database::StartWrite() {
      lock.Acquire();
      while ((AW+AR) > 0) {
          WW++;
          okToWrite.wait(%lock);
          WW--;
      }
      AW++;
      lock.Release();
  }
  
  Private Database::DoneWrite() {
      lock.Acquire();
      AW--;
      if (WW>0) {
          okToWrite.signal(); //只一个唤醒
      }
      else if (WR>0) {
          okToRead.broadcast(); //多个都唤醒
      }
      lock.Release();
  }
  ```

---

#### 哲学家就餐问题

+ 共享数据
  + Bowl of rice (data set)
  + Semaphore fork [5] initialized to 1

+ take_fork(i)：P(fork[i])
+ put_fork(i)：V(fork[i])

```c
#define N 5                   //哲学家个数
void philosopher(int i)       //哲学家编号0-4
    
    while(TRUE) {
        think();              //哲学家在思考
        take_fork(i);         //去拿左边的叉子
        take_fork((i+1)%N);   //去拿右边的叉子
        eat();                //吃面条中...
        put_fork(i);          //放下左边的叉子
        put_fork((i+1)%N);    //放下右边的叉子
    }
```

#### 不正确，可能导致死锁

```c
#define N 5                   //哲学家个数
void philosopher(int i)       //哲学家编号0-4
while(1)                      //去拿两把叉子
{
    take_fork(i);             //去拿左边的叉子
    if (fork((i+1)%N)) {      //右边叉子还在吗
        take_fork((i+1)%N);   //去拿右边的叉子
        break;                //两把叉子均到手
    } else {                  //右边叉子已不在
        put_fork(i);          //放下左边的叉子
        wait_some_time();     //等待一会儿
    }
}
```

#### 对拿叉子的过程进行了改进，但仍不正确

```c
#define N 5                   //哲学家个数
void philosopher(int i)       //哲学家编号0-4
while(1)                      //去拿两把叉子
{
    take_fork(i);             //去拿左边的叉子
    if (fork((i+1)%N)) {      //右边叉子还在吗
        take_fork((i+1)%N);   //去拿右边的叉子
        break;                //两把叉子均到手
    } else {                  //右边叉子已不在
        put_fork(i);          //放下左边的叉子
        wait_random_time();   //等待随机长时间
    }
}
```

#### 等待时间随机变化，可行，但非万全之策

```c
semaphore mutex               //互斥信号量，初值1
void philosopher(int i)       //哲学家编号0-4
    while(TRUE) {
        think();              //哲学家在思考
        P(mutex);             //进入临界区
        take_fork(i);         //去拿左边的叉子
        take_fork((i+1)%N);   //去拿右边的叉子
        eat();                //吃面条中...
        put_fork(i);          //放下左边的叉子
        put_fork((i+1)%N);    //放下右边的叉子
        V(mutex);             //退出临界区
    }
```

#### 互斥访问。正确，但每次只允许一人进餐

#### 该方法的缺点

+ 它把就餐（而不是叉子）看成是必须互斥访问的临界资源，因此会造成（叉子）资源的浪费
+ 从理论上说，如果有五把叉子，应允许两个不相邻的哲学家同时进餐

---

#### 思路（1）哲学家自己怎么来解决这个问题？

#### 指导原则：要么不拿，要么就拿起两把叉子

S1  思考中...
S2  进入饥饿状态
S3  如果左邻居或右邻居正在进餐，等待；否则转S4
S4  拿起两把叉子
S5  吃面条...
S6  放下左边的叉子
S7  放下右边的叉子
S8  新的循环又开始了，转S1

#### 思路（2）计算机程序怎么来解决这个问题？

#### 指导原则：不能浪费CPU时间；进程间相互通信

S1  思考中...
S2  进入饥饿状态
S3  如果左邻居或右邻居正在进餐，进程进入阻塞态；否则转S4
S4  拿起两把叉子
S5  吃面条...
S6  放下左边的叉子，看看左邻居现在能否进餐（饥饿状态，两把叉子都在），若能则唤醒之
S7  放下右边的叉子，看看右邻居现在能否进餐（饥饿状态，两把叉子都在），若能则唤醒之
S8  新的一轮又开始了，转S1

---

#### 思路（3）怎么样来编写程序？

1. 必须有数据结构，来描述每个哲学家的当前状态
2. 该状态是一个临界资源，各个哲学家对它的访问应该互斥地进行——进程互斥
3. 一个哲学家吃饱后，可能要唤醒它的左邻右舍，两者之间存在着同步关系——进程同步

---

1. 必须有一个数据结构，来描述每个哲学家的当前状态

   ```c
   #define N 5           //哲学家个数
   #define LEFT i        //第i个哲学家的左邻居
   #define RIGHT (i+1)%N //第i个哲学家的右邻居
   #define THINKING 0    //思考状态
   #define HUNGRY 1      //饥饿状态
   #define EATING 2      //进餐状态
   int state[N];         //记录每个人的状态
   ```

2. 该状态是一个临界资源，对它的访问应该互斥地进行

   ```c
   semaphore mutex;      //互斥信号量，初值1
   ```

3. 一个哲学家吃饱后，可能要唤醒邻居，存在着同步关系

   ```c
   semaphore s[N];       //同步信号量，初值0
   ```

---

#### 函数philosopher的定义

```c
void philosopher(int i)    //i的取值：0到N-1
{
    while(TRUE)            //封闭式循环
    {
        think();           //思考中...                 S1
        take_forks(i);     //拿到两把叉子或被阻塞        S2-S4
        eat();             //吃面条中...               S5
        put_forks(i);      //把两把叉子放回原处          S6-S7
    }
}
```

#### 函数take_forks的定义

```c
//功能：要么拿到两把叉子，要么被阻塞起来
void take_forks(int i)     //i的取值：0到N-1
{
    P(mutex);              //进入临界区
    state[i] = HUNGRY;     //我饿了
    test_take_left_right_forks(i);  //试图拿两把叉子
    V(mutex);              //退出临界区
    P(s[i]);               //没有叉子便阻塞
}
```

#### 函数test_take_left_right_forks的定义

```c
void test_take_left_right_forks(int i)    //i:0到N-1
{
    if(state[i] == HUNGRY &&              //i:我自己，or其他人
      state[LEFT] != EATING &&
      state[RIGHT] != EATING)
    {
        state[i] = EATING;                //两把叉子到手
        V(s[i]);                          //通知第i人可以吃饭了
    }
}
```

#### 函数put_forks的定义

```c
//功能：把两把叉子放回原处，并在需要的时候，去唤醒左邻右舍
void put_forks(int i)       //i的取值：0到N-1
{
    P(mutex);               //进入临界区
    state[i] = THINKING;    //交出两把叉子
    test_take_left_right_forks(LEFT);   //看左邻居能否进餐
    test_take_left_right_forks(RIGHT);  //看右邻居能否进餐
    V(mutex);               //退出临界区
}
```















---

# 一些问题

#### 唤醒和睡眠

线程的切换只在就绪队列中切换，唤醒和睡眠是将线程/进程置于就绪队列中

#### goto有害论























