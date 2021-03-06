# Redis

Redis是完全开源免费的，高性能的key-value**数据库**

#### Redis特点

+ Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用
+ Redis不仅仅支持简单的key-value类型的数据，同时还提供list、set、zset、hash等数据结构的存储
+ Redis支持数据的备份，即master-slave模式的数据备份

#### Redis优势

+ 性能极高，Redis能读的速度是110000次/s，写的速度是81000次/s
+ 丰富的数据类型，Redis支持二进制案例的Strings，Lists，Hashes，Sets即Ordered Sets数据类型操作
+ 原子，Redis的所有操作都是原子性的，意思就是要么成功执行要么失败完全不执行，单个操作是原子性的，多个操作也支持事务，即原子性，通过MULTI和EXEC指令包起来
+ 丰富的特性，Redis还支持publish/subscribe，通知，key过期等等特性

#### Redis与其他Key-Value存储有什么不同(不懂)

+ Redis有着更为复杂的数据结构并且提供对他们的原子性操作，这是一个不同于其他数据库的进化路径，Redis的数据类型都是基于数据结构的同时对程序员透明，无需进行额外的抽象
+ Redis运行在内存中但是可以持久化到磁盘，所以在对不同数据集进行高速读写时要权衡内存，因为数据量不能大于硬件内存，在内存数据库方面的另一个优点是，相比在磁盘上相同的复杂的数据结构，在内存中操作起来非常简单，这样Redis可以做很多内部复杂性很强的事情，同时，在磁盘格式方面他们是紧凑的以追加的方式产生的，因为他们并不需要进行随机访问

---

#### 使用方法(简易教程)

+ 下载redis：https://github.com/MicrosoftArchive/redis/releases
+ 解压
+ 解压路径运行cmd，启动redis服务端`.\redis-server.exe .\redis.windows.conf`
+ 运行客户端`.\redis-cli.exe`
+ 输入命令`lpush name 5`其中的name为自定义的名字

此时即可开始使用(python)：

```python
import redis, time

connection_pool = redis.ConnectionPool(host='127.0.0.1', db=0)
rcon = redis.Redis(connection_pool=connection_pool)
while True:
    task = rcon.rpop("name")
    if not task:
        time.sleep(1)
        continue
    # do some work...
```

