### CPU时间观

| 操作                       | 真实延迟 | CPU的感觉  |
| -------------------------- | -------- | ---------- |
| 执行指令                   | 0.38纳秒 | 1秒        |
| 读L1缓存                   | 0.5纳秒  | 1.3秒      |
| 分支纠错                   | 5纳秒    | 13秒       |
| 读L2缓存                   | 7纳秒    | 18.2秒     |
| 加/解互斥锁                | 25纳秒   | 1分5秒     |
| 内存寻址                   | 100纳秒  | 4分20秒    |
| 上下文切换/系统调用        | 1.5微秒  | 1小时5分钟 |
| 1Gbps网络上传输2KB数据     | 20微秒   | 14.4小时   |
| 从内存读1M连续数据         | 250微秒  | 7.5天      |
| Ping同IDC两台主机（来回）  | 0.5毫秒  | 15天       |
| 从SSD读1M连续数据          | 1毫秒    | 1个月      |
| 从硬盘读1M连续数据         | 20毫秒   | 20个月     |
| Ping不同城市的主机（来回） | 150毫秒  | 12.5年     |
| 虚拟机重启                 | 4秒      | 300年      |
| 服务器重启                 | 5分钟    | 2万5千年   |

假设一条指令是1秒钟，则千兆网上读取2KB数据，CPU感觉度过了14个小时，10M的公网上将会降低100倍

L1一级高速缓存：CPU内部的缓存

**C10k/C10M挑战**：C10k是concurrently handing 10k connections，在1999年，如何在1GHz CPU，2G内存，1Gbps网络环境下，让单台服务器同时为1万个客户端提供FTP服务，2010年，延伸为C10M，如何利用8核心CPU，64G内存，在10Gbps的网络上保持1000万并发连接，或是每秒钟处理100万的连接

### 同步阻塞——多进程——多线程

+ **进程切换开销**：CPU从一个进程切换到另一个进程，需要把旧进程运行的寄存器状态、内存状态全部保持好，再将另一个进程之前保存的数据恢复，进程数量大于CPU核心数时，进程切换是必须的
+ **多线程**：Python的多线程由于GIL的存在，不能利用CPU多核优势。同时还有线程之间的竞争问题，调度策略是抢占式的
+ **事件循环+回调**：需要程序员直接使用epoll去注册事件和回调、维护一个事件循环，然后大多数时间都花在设计回调函数上，但是存在栈撕裂(中间一环报错)和状态共享困难

### 异步编程的一些对象：

+ **Future**，未来对象，不用回调的方式，如何知道异步调用的结果？
  ***非链式调用***
  在异步调用执行完的时候，就把结果放在future里面，用于存放未来的执行结果，有一个set_result()方法，在给result绑定值后会调用事先给future添加的回调
  ***没太懂***

  ```python
  class Future:
      def __init__(self):
          self.result = None
          self._callbacks = []
      
      def add_done_callback(self, fn):
          self._callbacks.append(fn)
      
      def set_result(self, result):
          self.result = reslut
          for fn in self._callbacks:
              fn(self)
  ```

+ **Task**，任务对象，管理生成器的状态

  ```python
  class Task:
      def __init__(self, coro):
          self.coro = coro
          f = Future()
          f.set_result(None)
          self.step(f)
      
      def step(self, future):
          try:
              next_future = self.coro.send(future.result)
          except StopIteration:
              return
          # 在之后完成了之后selector回调会设置future.result，会执行此方法
          next_future.add_done_callback(self.step)
  ```

+ **Event Loop**，事件循环，等待已经注册的**EVENT_WRITE**事件发生

  ```python
  def loop()：
  	# 现在已经修改了一些，修改为回调不关心是谁触发了事件
      # 一个循环，去询问selector模块，等待他告诉我们当前是哪个事件发生了，应该对应哪个回调
      while not stopped:
          # selector.select()是一个阻塞调用，如果事件不发生，则应用程序就会等待在这里，阻塞并等待事件发生
          events = selector.select()
          for event_key, event_mask in events:
              callback = event_key.data
              callback()
  
  if __name__ == '__main__':
      import time
      start = time.time()
      for url in urls_todo:
          crawler = Crawler(url)  # 爬虫的协程对象crawler.fetch()
          Task(crawler.fetch())  # 启动了协程对象，并执行到第一次field返回值
      loop()
      print(time.time() - start)
  ```

  

这里的**Task**对象首先要在初始化的时候启动生成器，即**send(None)**
send()之后会得到下一次的future，然后为下一次的future添加了step()回调，即step()
生成器里面注册的selector的回调很简单，就是给future对象绑定结果值