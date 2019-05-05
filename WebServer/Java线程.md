### 线程

Java的线程有五个状态

**初始化、可运行、运行中、阻塞、销毁**

由JVM通过操作系统内核中的TCB（Thread Control Block）模块来改变线程的状态，这一过程需要消耗CPU资源





# 协程

协程，Coroutines，是一种比线程更加轻量级的存在，一个线程可以拥有多个协程

详见asyncio&aiohttp.md