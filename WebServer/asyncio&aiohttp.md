## 基本思想

**多进程、多线程、协程、异步**

对于操作系统来说，线程是最小的执行单元，进程是最小的资源管理单元

**并发、并行**
并行是多个事件在同一时刻进行，并发是多个事件在同一时间段进行

多线程切换有**上下文切换Context Switch时间损耗**，导致性能损失
python可能是少数仅有的**支持多线程的解释型语言**，Ruby也支持多线程，如PHP是没有多线程的

代码是CPU密集型的时候，Python多个线程的代码很可能是线性执行的，多线程变得鸡肋，因为Context Switch

CPU密集型和IO密集型

+ CPU密集型，**CPU bound**：
  计算占很大比重，IO时间很短，CPU Loading很高，即大部分时间用来做计算、逻辑判断等CPU动作的程序称为CPU bound
+ I/O密集型，**IO bound**：
  系统大部分的状况是CPU在等I/O(包括磁盘、内存、网络)的读写操作，此时CPU Loading并不高

**计算密集型的任务同时进行的数量应该等于CPU的核心数**，不适合脚本语言，使用C语言最佳，对于Python语言即使是多核心，计算密集型任务也约等于单线程，甚至不如单线程，因为GIL锁

常见的大部分任务都是I/O密集型，对于IO密集型任务，最合适的语言就是开发效率最高，即代码量最少的语言，脚本语言是首选，C语言最差

*Python要写CPU密集型代码可以使用concurrent，**multiprocessing**库，但是我没有了解过的*



#### GIL的概念需要知道

***GIL***全称为Global Interpreter Lock（全局解释器锁）
来源于python设计之初，为了数据的安全所做的决定。其实是实现python解释器CPython时候引入的一个概念，不同的解释器可以不依赖于GIL，只是CPython是大部分环境下默认的Python执行环境，而JPython就没有GIL

python多线程环境下，每个线程的执行方式为：

+ 获取GIL
+ 执行代码直到sleep或者是python将其挂起
+ 释放GIL

GIL只有一个，线程只有拿到GIL才能够执行
**GIL释放时机**：通过当前线程遇见IO操作或者是ticks计数（可以认为是python本身的一个计数器，python2使用）到达100，这个值可以通过sys.setcheckinterval来调整，python3中使用计时器，时间到达阈值后自动释放
多核CPU中python的多线程效率并不高，同一时间只能执行一个线程的问题，同时**线程颠簸thrashing**，即某个线程刚释放GIL之后，由于释放和获得GIL的时间特别短暂，很多情况下自己又拿到了GIL，其他核心上被唤醒的线程会白白浪费CPU时间，到达切换时间后又进入待调度状态，再被唤醒，再等待，导致效率更低
即其他核心的线程会：被唤醒、切换时间段、进入待调度状态、被唤醒...

Python在多线程的多核CPU下，只对于IO密集型计算产生正面效果；而当至少有一个CPU密集型线程存在，那么多线程的效率会由于GIL而大幅下降，因为IO线程引起切换之后，CPU密集型线程会继续占有GIL锁，从而进入循环等待



#### 总结

Python GIL是功能和性能之间权衡后的产物，IO Bound场景下的多线程会得到较好的性能，如果对并行计算性能较高的程序，可以考虑把核心部分换成C模块，或者是用其他语言实现





# 协程

协程，Coroutines，是一种比线程更加轻量级的存在，一个线程可以拥有多个协程

Java中原生语法中没有实现协程，某些开源框架实现了协程，但是很少被使用。以python中对协程的实现为例

主要使用yield语法，协程执行到yield会暂停在那一行，等到主线程调用了send方法发送了数据，协程才会接到数据继续执行

协程暂停不同于线程阻塞，协程暂停由程序控制，线程阻塞由操作系统内核来进行切换，**协程的开销远小于线程的开销**

Lua语言
Python语言：**yield/send**实现和**async/await**实现
Go语言：可以轻松的创建上百上千个协程并发执行
Java语言：小众的一个框架



## asyncio异步I/O

python3标准库
python3.5新增**async**/**await**
***百万并发***

磁盘I/O和**网络I/O**

**asyncio**是用来编写并发代码的库，使用**async**/**await**语法
**asyncio**被用作多个提供高性能Python异步框架的基础，包括网络或**网站服务**，**数据库链接库**、**分布式任务队列**等等

asyncio往往是构建IO密集型和高层级结构化网络代码的最佳选择

```python
import asyncio

async def main():
    print('Hello')
    await asyncio.sleep(1)
    print('...World!')
    
# python 3.7+
asyncio.run(main())
```

asyncio提供一组**高层级API**用于
1、并发地**运行Python协程**并对其执行过程实现完全控制
2、执行**网络IO**和**IPC**(进程间通信)
3、控制子进程
4、通过队列实现分布式任务
5、同步并发代码

还有一些**低层级API**以支持库和框架的开发者实现
1、创建和管理事件循环，以提供异步API用于网络化，运行子进程，处理**OS信号(操作系统)**等
2、使用transports实现高效率协议
3、通过async/await语法桥接基于回调的库和代码

---

#### asyncio基础概念

**`event_loop()`事件循环**：
程序开启一个无限的循环，程序员会把一些函数注册到事件循环上，当满足事件发生的时候，调用相应的协程函数

**`coroutine`协程**：
协程对象，指一个使用async关键字定义的函数，它的调用不会立即执行函数，而是会返回一个协程对象。协程对象需要注册到事件循环，由事件循环调用

**`future`对象**：
代表将来执行或没有执行的任务的结果，和Task么有本质的区别

**`task`任务**：
一个协程对象就是一个原生可以挂起的函数，任务则是对协程进一步封装，其中包含任务的各种状态。Task对象是Future的子类，它将coroutine和Future联系在一起，将coroutine封装成一个Future对象

**`async/await`关键字**：
python3.5用于定义协程的关键字，async定义一个协程，await用于挂起阻塞的异步调用接口，其作用在一定程度上类似yield，即暂停效果
但不能在生成器中使用await，也不能在async定义的协程中使用yield

---

#### 协程完整的工作流程

+ 定义/创建协程对象
+ 将协程转化为task任务
+ 定义事件循环对象容器
+ 将task任务扔进事件循环对象中触发

```python
import asyncio

async def hello(name):
    print('Hello', name)

coroutine = hello('World')
loop = asyncio.get_event_loop()
# task = asyncio.ensure_future(coroutine)  也可以实现将协程转化为task对象
task = loop.create_task(coroutine)

loop.run_until_complete(task)
```

创建task之后，task在加入事件循环loop之前是pending状态，task执行完会变成finished状态

`asyncio.ensure_future(corcoutine)`和`loop.create_task(coroutine)`都可以创建一个task
`run_until_complete`的参数是一个future对象，当传入一个协程，其内部会自动封装成一个task，task是future的子类

---

#### 绑定回调函数

异步IO实现的原理，就是再IO高的地方挂起，等IO结束后，再继续执行，再绝大部分时候，后续代码的执行是需要依赖IO的返回值的，这时候就需要回调了

两种实现回调的方式：

+ 一种是利用同步编程实现的回调，`task.result()`可以取得返回结果，即在任务结束之后，使用`task.result()`方法可以得到task的返回值
+ 还有一种是通过asyncio自带的**添加回调函数**功能来实现，回调的最后一个参数是future对象，通过该对象可以获取协程返回值，如果回调需要多个参数，可以通过偏函数导入(偏函数即固定某些参数的方法)

```python
import time
import asyncio

async def _sleep(x):
    time.sleep(2)
    return '暂停了{}秒'.format(x)

def callback(future):
    print('这里是回调函数，获取返回结果是：', future.result())

coroutine = _sleep(2)
loop = asyncio.get_event_loop()
task = asyncio.ensure_future(coroutine)
task.add_done_callback(callback)

loop.run_until_complete(task)
"""
# 偏函数的用法
def callback(t, future):
    print('Callback:', t, future.result())

task.add_done_callback(functools.partial(callback, 2))
# 此时2会作为第一个参数传入callback并作为一个新的函数
# 这里的task和future可以认为是一样的
"""
```

---

#### 阻塞和await

使用async可以定义协程对象，使用await可以针对耗时的操作进行挂起，就像生成器里的yield一样，函数让出控制权。协程遇到await，事件循环将会挂起该协程，执行别的协程，直到其他的协程也挂起或者执行完毕，再进行下一个协程的执行。耗时操作一般是网络IO，磁盘IO等，协程的目的也是让这些IO操作异步化

当遇到阻塞调用的函数的时候，使用await方法将协程的控制权让出，以便loop调用其他的协程

---

#### 并发和并行

asyncio实现并发，需要多个协程来完成任务，每当有任务阻塞的时候就await，然后其他协程继续工作。创建多个协程的列表，然后将这些协程注册到事件循环中

两种注册手段：**`asyncio.wait(tasks)`**和**`asyncio.gather(*tasks)`**

```python
import asyncio

async def do_some_work(x):
    print('Waiting:', x)
    await asyncio.sleep(x)
    return 'Done after {}s'.format(x)

coroutine1 = do_some_work(1)
coroutine2 = do_some_work(2)
coroutine3 = do_some_work(4)
tasks = [
    asyncio.ensure_future(coroutine1),
    asyncio.ensure_future(coroutine2),
    asyncio.ensure_future(coroutine3)
]
loop = asyncio.get_event_loop()
# asyncio.wait(tasks)也可以使用asyncio.gather(*tasks)
# 前者接受一个task列表，后者接受一堆task
loop.run_until_complete(asyncio.wait(tasks))
for task in tasks:
    print('Task ret: ', task.result())
```

---

#### 协程嵌套

使用async可以定义协程，协程用于耗时的IO操作，我们可以封装更多的IO操作过程，这样就实现了嵌套的协程，即一个协程中await了另外一个协程，如此连接起来

```python
import asyncio

async def do_some_work(x):
    print('Waiting: ', x)
    await asyncio.sleep(x)
    return 'Done after {}s'.format(x)

async def main():
    coroutine1 = do_some_work(1)
    coroutine2 = do_some_work(2)
    coroutine3 = do_some_work(4)
    tasks = [
        asyncio.ensure_future(coroutine1),
        asyncio.ensure_future(coroutine2),
        asyncio.ensure_future(coroutine3)
    ]
    # asyncio.wait(tasks)
    dones, pendings = await asyncio.wait(tasks)
    for task in dones:
        print('Task ret: ', task.result())
    # asyncio.gather(*tasks)
    # results = await asyncio.gather(*tasks)
    # for result in results:
    #     print('Task ret: ', result)
        
loop = asyncio.get_event_loop()
loop.run_until_complete(main())
```

只是把创建协程对象，转换task任务，封装成在一个协程函数里而已，外部的协程，嵌套了一个内部的协程

不在main协程函数里处理结果，直接返回await的内容，那么最外层的run_until_complete将会返回main协程的结果

```python
async def main():
    coroutine1 = do_some_work(1)
    coroutine2 = do_some_work(2)
    coroutine3 = do_some_work(4)
    tasks = [
        asyncio.ensure_future(coroutine1),
        asyncio.ensure_future(coroutine2),
        asyncio.ensure_future(coroutine3)
    ]
    return await asyncio.gather(*tasks)

loop = asyncio.get_event_loop()
results = loop.run_until_complete(main())
for result in results:
    print('Task ret: ', result)
```

使用asyncio.wait方式挂起协程

```python
async def main():
    coroutine1 = do_some_work(1)
    coroutine2 = do_some_work(2)
    coroutine3 = do_some_work(4)
    tasks = [
        asyncio.ensure_future(coroutine1),
        asyncio.ensure_future(coroutine2),
        asyncio.ensure_future(coroutine3)
    ]
    return await asyncio.wait(tasks)

loop = asyncio.get_event_loop()
done, pending = loop.run_until_complete(main())
for task in done:
    print('Task ret: ', task.result())
```

也可以使用asyncio的as_completed方法

```python
async def main():
    coroutine1 = do_some_work(1)
    coroutine2 = do_some_work(2)
    coroutine3 = do_some_work(4)
    tasks = [
        asyncio.ensure_future(coroutine1),
        asyncio.ensure_future(coroutine2),
        asyncio.ensure_future(coroutine3)
    ]
    # 会一个一个打印出来，第1秒，第2秒，第4秒
    for task in asyncio.as_completed(tasks):
        result = await task
        print('Task ret: {}'.format(result))
    # 不太清楚as_completed(tasks)怎么用的

loop = asyncio.get_event_loop()
done = loop.run_until_complete(main())
```

协程的调用和组合十分灵活，尤其是对于结果的处理，如何返回，如何挂起，需要逐渐积累经验和前瞻的设计

---

#### 协程停止

future的状态
**Pending**、**Running**、**Done**、**Cancelled**

创建future的时候，task为Pending，事件循环调用执行的时候当然是running，调用完就是Done。如果需要停止事件循环，就需要先把task取消。可以使用`asyncio.Task`获取事件循环的task

```python
import asyncio

async def do_some_work(x):
    print('Waiting: ', x)
    await asyncio.sleep(x)
    return 'Done after {}s'.format(x)

coroutine1 = do_some_work(1)
coroutine2 = do_some_work(2)
coroutine3 = do_some_work(4)
tasks = [
    asyncio.ensure_future(coroutine1),
    asyncio.ensure_future(coroutine2),
    asyncio.ensure_future(coroutine3)
]
loop = asyncio.get_event_loop()
try:
    loop.run_until_complete(asyncio.wait(tasks))
except KeyboardInterrupt as e:
    print(asyncio.Task.all_tasks())
    for task in asyncio.Task.all_tasks():
        # True表示cancel成功
        print(task.cancel())
    loop.stop()
    # loop.stop()之后还需要再次开启事件循环，最后再close，不然会抛出异常
    loop.run_forever()
finally:
    loop.close()
```

循环task，逐个cancel是一种方案，当task的列表封装再main函数中，main函数外进行事件循环的调用，这个时候，main相当于最外层的一个task，那么处理包装的main函数即可

```python
import asyncio

async def do_some_work(x):
    print('Waiting: ', x)
    await asyncio.sleep(x)
    return 'Done after {}s'.format(x)

async def main():
    coroutine1 = do_some_work(1)
	coroutine2 = do_some_work(2)
	coroutine3 = do_some_work(4)
	tasks = [
    	asyncio.ensure_future(coroutine1),
	    asyncio.ensure_future(coroutine2),
    	asyncio.ensure_future(coroutine3)
	]
    done, pending = await asyncio.wait(tasks)
    for task in done:
        print('Task ret: ', task.result())

loop = asyncio.get_event_loop()
task = asyncio.ensure_future(main())
try:
    loop.run_until_complete(task)
except KeyboardInterrupt as e:
    print(asyncio.Task.all_tasks())
    print(asyncio.gather(*asyncio.Task.all_tasks()).cancel())
    loop.stop()
    loop.run_forever()  # 有疑问，这里应该会一直的等待下去，不会去执行close
finally:
    loop.close()
```

---

#### 不同线程的事件循环

很多时候，我们的事件循环用于注册协程，而有的协程需要动态的添加到事件循环中，一个简单的方式就是使用多线程。当前线程创建一个事件循环，然后再新建一个线程，再新线程中启动事件循环，当前线程不会被block

```python
import asyncio
from threading import Thread

def start_loop(loop):
    asyncio.set_event_loop(loop)
    loop.run_forever()
    
async def do_some_work(x):
    print('Waiting {}'.format(x))
    await asyncio.sleep(x)
    print('Done after {}s'.format(x))

def more_work(x):
    print('More work {}'.format(x))
    time.sleep(x)
    print('Finished more work {}'.format(x))
    
new_loop = asyncio.new_event_loop()
t = Thread(target=start_loop, args=(new_loop,))
t.start()

# new_loop.call_soon_threadsafe(more_work, 6)
# new_loop.call_soon_threadsafe(more_work, 3)
asyncio.run_coroutine_threadsafe(do_some_work(6), new_loop)
asyncio.run_coroutine_threadsafe(do_some_work(4), new_loop)
```

`call_soon_threadsafe()`方法添加非协程函数
`run_coroutine_threadsafe()`方法添加协程函数

---

#### gather与wait

把多个协程注册进一个事件循环中的两种方法：

+ `asyncio.wait(tasks)`这里的`task`必须是一个list，这个list存放多个task，可以使用`asyncio.ensure_future()`转化为task对象，也可以不转，直接传入协程对象的list

+ `asyncio.gather(*tasks)`这里的`*`不能省略，可以接收list，`gather()`第一个参数是`*coros_or_futures`，叫***非命名键值可变长参数列表***，可以集合所有没有命名的变量

  ```python
  loop = asyncio.get_event_loop()
  loop.run_until_complete(asyncio.gather(
      do_some_work(1),
      do_some_work(2),
      do_some_work(3),
  ))
  # 或者
  loop = asyncio.get_event_loop()
  group1 = asyncio.gather(*[do_some_work(x) for x in range(1, 3)])
  group2 = asyncio.gather(*[do_some_work(x) for x in range(1, 5)])
  group3 = asyncio.gather(*[do_some_work(x) for x in range(1, 7)])
  loop.run_until_complete(asyncio.gather(group1, group2, group3))
  ```

两种方法返回结果也不一样
`asyncio.wait()`返回`dones`和`pendings`；`asyncio.gather()`会把值直接返回

此外`asyncio.wait()`还有**控制功能**：

```python
...
# FIRST_COMPLETED:第一个任务完成返回
# FIRST_EXCEPTION:产生第一个异常返回
# ALL_COMPLETED:默认选项，所有任务完成返回
dones, pendings = loop.run_until_complete(
    asyncio.wait(tasks, return_when=asyncio.FIRST_COMPLETED))
# asyncio.wait(tasks, timeout=1)  # 运行1秒后返回
```

---

#### master-worker主从模式

对于并发任务，通常是用**生成消费**模型，对队列的处理可以使用类似master-worker的方式，master主要用户获取队列的msg，worker用户处理消息

---

#### 工具函数

`asyncio.sleep()`该函数为asyncio自带的工具函数，可以模拟IO阻塞，返回一个协程对象



***yield和async使用注意***

+ 不能再生成器中使用await，也不能再async定义的协程中使用yield
+ `yield from`后可以接**可迭代对象**，**生成器**，**迭代器**，也可以接**future对象**/**协程对象**，但是await后面必须接**future对象**/**协程对象**