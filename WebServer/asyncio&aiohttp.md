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

#### 低层级应用

##### 事件循环

事件循环是每个asyncio应用的核心，事件循环会运行异步任务和回调，执行网络IO操作，以及运行子进程

应用开发者通常应当使用高层级的asyncio函数，例如`asyncio.run()`，他们应当很少有必要引用循环对象或调用其方法。

**获取事件循环**
获取、设置、创建事件循环

| API                        | 描述                                   |
| -------------------------- | -------------------------------------- |
| asyncio.get_running_loop() | 获取当前运行的事件循环的首选函数       |
| asyncio.get_event_loop()   | 获得一个事件循环实例(当前或通过策略)   |
| asyncio.set_event_loop()   | 通过当前策略将事件循环设置当前事件循环 |
| asyncio.new_event_loop()   | 创建一个新的事件循环                   |

`asyncio.get_running_loop()`返回当前OS线程中正在运行的事件循环
如果没有正在运行的事件循环则会引发RuntimeError。此函数只能有协程或回调来调用

`asyncio.get_event_loop()`获取当前事件循环
如果当前OS线程没有设置当前事件循环并且`set_event_loop()`还没有被调用，将创建一个新的事件循环并将其设置为当前循环
猜测将等于`new_event_loop()`并`set_event_loop(loop)`

`asyncio.set_event_loop(loop)`将loop设置为当前OS线程的当前事件循环

`asyncio.new_event_loop()`创建一个新的事件循环

**运行和停止循环**
`loop.run_until_complete(future)`运行直到future(Future的实例)被完成
如果参数是`coroutine object`，将被隐式调度为`asyncio.Task`来运行
返回Future的结果或者是引发相关异常

`loop.run_forever()`运行事件循环直到`stop()`被调用

`loop.stop()`停止事件循环

`loop.is_running()`返回True如果事件循环当前正在运行

`loop.is_closed()`如果事件循环已经被关闭，返回True

`loop.close()`关闭事件循环
调用时，循环必须处于非运行状态，pending状态的回调将被丢弃，此方法清除所有的队列并立即关闭执行器，不会等待执行器完成

`loop.shutdown_asyncgens()`

**调度回调**
`loop.call_soon(callback, *args, context=None)`安排在下一次事件循环的迭代中使用args参数调用callback

`loop.call_soon_threadsafe(callback, *args, context=None)`必须被用于安排来自其他线程的回调











***



```python
async def hello():
    asyncio.sleep(1)
    print('HelloWorld!')

loop = asyncio.get_event_loop()
loop.run_until_complete(hello())
```

`async def` 用来定义异步函数，其内部结构有异步操作。主线程调用`asyncio.get_event_loop()`会创建***事件循环loop***，需要把异步的任务丢给这个循环loop的`run_until_complete()`方法，loop会安排协程执行。协程通过async/await语法进行声明，是编写异步应用的推荐方式



真正运行一个协程，asyncio提供了**两种主要机制**：

1. `asyncio.run()`用来运行最高层级的入口点`main()`函数

   ```python
   import asyncio
   import time
   
   async def say_after(delay, what):
       await asyncio.sleep(delay)
       print(what)
       
   async def main():
       print(f"started at {time.strftime('%X')}")
       await say_after(1, 'hello')
       await say_after(2, 'world')
       print(f"finished at {time.strftime('%X')}")
   
   asyncio.run(main())
   ```

   输出结果

   ```
   started at 19:38:27
   hello
   world
   finished at 19:38:30
   ```

2. `asyncio.create_task()`函数来并发运行作为asyncio任务的多个协程

   ```python
   async def main():
       task1 = asyncio.create_task(say_after(1, 'hello'))
       task2 = asyncio.create_task(say_after(2, 'world'))
       print(f"started at {time.strftime('%X')}")
       await task1
       await task2
       print(f"finished at {time.strftime('%X')}")
   ```

   ```
   started at 19:45:22
   hello
   world
   finished at 19:45:24
   ```



如果一个对象可以在await语句中使用，那么他就是**可等待对象**
可等待对象主要有三种类型：**协程**、**任务**、**Future**

***协程：***
协程函数，定义形式为`async def`的函数
协程对象，调用*协程函数*所返回的对象
协程对象必须被`await`
asyncio也支持旧式的**基于生成器**的协程

```python
import asyncio

async def nested():
    return 42

async def main():
    # Nothing happens if we just call "nested()".
    # A coroutine object is created but bot awaited,
    # so it *won't run at all*.
    nested()
    
    # Let's do it differently now and await it:
    print (await nested())  # will pritn "42".

asyncio.run(main())
```



***任务：***
任务被用来设置日程以便并发执行协程
一个协程通过`asyncio.create_task()`等函数被打包为一个任务，该协程将自动排入日程准备立即运行

```python
import asyncio

async def nested():
    return 42

async def main():
    # Schedule nested() to run soon concurrently
    # with "main()".
    task = asyncio.create_task(nested())
    
    # "task" can now be used to cancel "nested()", or
    # can simply be awaited to wait until it is complete:
    await task
    
asyncio.run(main())
```



***Future对象：***（没懂）
是一种特殊的低层级可等待对象，表示一个异步操作的最终结果
当一个Future对象被等待，这意味着协程将保持等待直到给Future对象在其他地方操作完毕
在asyncio中需要Future对象以便允许通过async/await使用基于回调的代码
通常情况下没有必要再应用层级的代码中创建Future对象
Future对象有时候会由库和某些asyncio API暴露给用户，用作可等待对象

```python
async def main():
    await function_that_returns_a_future_obbject()
    
    # this is also valid:
    await asyncio.gather(
        function_that_returns_a_future_object(),
        some_python_coroutine()
    )
```

一个很好的返回对象的低层级函数的示例是`loop.run_in_executor()`



#### 运行asyncio程序

```python
asyncio.run(coro, *, debug=False)
```

此函数运行传入的协程，负责管理asyncio事件循环并完结异步生成器
**当还有其他asyncio事件循环在同一线程中运行时，此函数不能被调用（没懂）**
如果debug为True，事件循环将以调试模式运行



#### 创建任务

```python
# In Python 3.7+
task = asyncio.create_task(coro())
# This works in all Python versions but is less readable
task = asyncio.ensure_future(coro())
```

将coro协程打包为一个Task排入日程准备执行，返回Task对象
该任务会在get_running_loop()返回的循环中执行，如果当前线程没有在运行的循环则会引发RuntimeError



#### 休眠

```python
import asyncio
import datetime

async def display_data():
    loop = asyncio.get_running_loop()
    end_time = loop.time() + 5.0
    while True:
        print(datetime.datetime.now())
        if (loop.time() + 1.0) >= end_time:
            break
        await asyncio.sleep(1)

asyncio.run(display_date())

# asyncio.sleep(delay, result=None, *, loop=None)
```

阻塞delay指定的秒数
制定了result则在协程完成时将其返回给调用者
sleep()总会挂起当前任务，以允许其他任务运行，loop参数已弃用

#### 并发运行任务

```python
import asyncio

async def factorial(name, number):
    f = 1
    for i in range(2, number + 1):
        print(f"Task {name}: Compute factorial({i})...")
        await asyncio.sleep(1)
        f *= i
    print(f"Task {name}: factorial({number}) = {f}")

async def main():
    # Schedule three calls *concurrently*:
    await asyncio.gather(
        factorial("A", 2),
        factorial("B", 3),
        factorial("C", 4),
    )
    
asyncio.run(main())

# asyncio.gather(*aws, loop=None, return_exceptions=False) 
```

并发运行aws序列中的可等待对象
如果aws中的某个可等待对象为协程，它将自动作为一个任务加入日程
如果所有可等待对象都成功完成，结果将是一个由所有返回值聚和而成的列表，结果值的顺序和aws中可等待对象的顺序一致
如果return_exceptions为False（默认），所引发的首个异常会立即传播给等待gather()的任务，aws序列中的其他可等待对象不会被取消并将继续运行；为True，异常会和成功的结果一样处理，并聚合至结果列表
如果gather()被取消，所有被提交(尚未完成)的可等待对象也会被取消



#### 屏蔽取消操作

```python
asyncio.shield(aw, *, loop=None)
```

保护一个可等待对象防止其被取消
如果aw是一个协程，将自动作为任务加入日程



#### 超时

```python
asyncio.wait_for(aw, timeout, *, loop=None)
```

等待aw可等待对象完成，指定timeout秒数后超时
timeout为None，则等待直到完成
超时时，任务将取消并引发`asyncio.TimeoutError`
要避免任务取消，可以加上`shield()`
loop参数已弃用



#### 基于生成器的协程

对基于生成器的协程的支持***已弃用***，并计划在Python3.10中移除。基于生成器的协程是async/await语法的前身，它们使用`yield from`语句创建的Python生成器可以等待Future和其他协程，`@asyncio.coroutine`装饰基于生成器的协程

## aiohttp

基于asyncio的http框架