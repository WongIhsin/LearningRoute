#### yield的用法

如果一个函数定义中包含了**yield**关键字，那么这个函数就不再是一个普通函数，而是一个generator

generator的函数，每次调用**next()**的时候执行，遇到**yield**语句返回，再次执行时从上次返回的**yield**语句处继续执行

```python
def consumer():
    r = 'F'
    print('A')
    while True:
        print('yield')
        n = yield r
        print('N', n)
        if not n:
            return
        print('CONSUMER Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    print(c.send(None))
    n = 0
    while n < 5:
        n += 1
        print('PRODUCE Producing %s...' % n)
        r = c.send(n)
        print('PROCUCER Consumer return: %s' % r)
    c.close()

c = consumer()
produce(c)
```

执行到yield会切换

控制台输出如下

```
A
yield
F
PRODUCE Producing 1...
N 1
CONSUMER Consuming 1...
yield
PROCUCER Consumer return: 200 OK
PRODUCE Producing 2...
N 2
CONSUMER Consuming 2...
yield
PROCUCER Consumer return: 200 OK
PRODUCE Producing 3...
N 3
CONSUMER Consuming 3...
yield
PROCUCER Consumer return: 200 OK
PRODUCE Producing 4...
N 4
CONSUMER Consuming 4...
yield
PROCUCER Consumer return: 200 OK
PRODUCE Producing 5...
N 5
CONSUMER Consuming 5...
yield
PROCUCER Consumer return: 200 OK
```

#### asyncio

asyncio是一个消息循环，需要执行的协程写入EventLoop即可实现异步IO

```python
import asyncio

@asyncio.coroutine
def hello():
    print('Hello world!')
    r = yield from asyncio.sleep(1)
    print('Hello again!')

loop = asyncio.get_event_loop()
tasks = [hello(), hello()]
loop.run_until_complete(asyncio.wait(tasks))
loop.close()
```

**asyncio**提供的**@asyncio.coroutine**把一个generator标记为coroutine类型，然后把这个coroutine扔到EventLoop中执行，在coroutine内部用yield from调用另一个coroutine实现异步操作

yield from语法可以方便地调用另一个generator，即asyncio.sleep(1)，但asyncio.sleep(1)是一个coroutine，所以线程不会等待，而是直接中断执行下一个消息循环

当asyncio.sleep(1)返回时，线程可以从yield from拿到返回值，然后接着执行下一行语句

python3.5引入新语法**async**和**await**

```python
@asyncio.coroutine
def hello():
    print('Hello world!')
    r = yield from asyncio.sleep(1)
    print('Hello again!')
```

```python
async def hello():
    print('Hello world!')
    r = await asyncio.sleep(1)
    print('Hello again!')
```

HTTP连接可以使用单线程+**coroutine**实现多用户的高并发支持

asyncio实现了TCP、UDP、SSL等协议，**aiohttp**则是基于asyncio实现的HTTP框架

