# Log4j（可以使用log4j2）

Apache旗下的一个开源项目Log4j，由Java编写的，可靠、灵活的日志框架。如今log4j已经被移植到了C、C++、Python等语言中

Log4j主要由三个重要组件构成：

+ Logger：日志对象，负责捕捉日志记录信息
  用来取代System.out或者System.err的日志输出器，负责日志信息的输出
+ Appender：日志输出目的地，负责把格式好的日志信息输出到指定地方，可以是控制台、磁盘文件等
  每个日志对象，都有一个对应的appender，每个appender代表着一个日志输出目的地：ConsoleAppender控制台、FileAppender磁盘文件、DailyRollingFileAppender每天产生一个日志磁盘文件、RollingFileAppender日志磁盘文件大小达到指定尺寸时产生一个新的文件
+ Layout：日志格式化器，负责发布不同风格的日志信息
  每个appender和一个Layout相对于，appender负责把日志信息输出到指定的地点，而Layout则负责把日志信息按照格式化的要求展示出来。HTMLLayout以html表格形式布局展示；PatternLayout自定义指定格式展示；SimpleLayout包含日志信息级别和信息字符串；TTCCLayout包含日志产生的时间、线程、类别等信息

---

#### 使用方法

首先需要在pom.xml中添加依赖

```xml
<!-- https://mvnrepository.com/artifact/log4j/log4j -->
<dependency>
    <groupId>log4j</groupId>
    <artifactId>log4j</artifactId>
    <version>1.2.17</version>
</dependency>
```

测试代码

```java
public class log4jDemo {
    Logger log = Logger.getLogger(log4jDemo.class);
    @Test
    public void test() {
        log.trace("Trace Message!");
        ...
    }
}
```

在classpath下声明配置文件：log4j.properties或者log4j.xml

```properties
log4j.rootLogger = INFO, FILE, CONSOLE
# [level] appenderName appenderName ...

log4j.appender.FILE = org.apache.log4j.FileAppender
log4j.appender.FILE.File = F:/log.out
log4j.appender.FILE.ImmediateFlush = true
log4j.appender.FILE.Threshold = DEBUG
log4j.appender.FILE.Append = true
log4j.appender.FILE.layout = org.apache.log4j.PatternLayout
log4j.appender.FILE.layout.conversionPattern = %d{ABSOLUTE} %5p %c{1}:%L - %m%n

log4j.appender.CONSOLE = org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.Target = System.out
log4j.appender.CONSOLE.ImmediateFlush = true
log4j.appender.CONSOLE.Threshold = DEBUG
log4j.appender.CONSOLE.layout = org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.encoding = UTF-8
log4j.appender.CONSOLE.layout.conversionPattern = %d{ABSOLUTE} %5p %c{1}:%L - %m%n
```

Log4j建议只使用4个级别，从高到低分别是ERROR>WARN>INFO>DEBUG

除了配置完log4j的配置文件，还要再applicationContext初始化的时候加载它的配置文件，再Spring框架中，需要在web.xml中进行简单设置

```xml
<!-- 设置log4j配置文件 -->
<context-param>
    <param-name>log4jConfigLocation</param-name>
    <param-value>classpath:log4j.properties</param-value>
</context-param>
<!-- log4j的初始化类 -->
<listener>
	<listener-class>
        org.springframework.web.util.Log4jConfigListener
    </listener-class>
</listener>
```

注意需要将log4j.properties放置在/src/main/resources目录下

或者指定文件位置

```java
//PropertyConfigurator.configure("conf/log4j.properties");
//DOMConfigurator.configure("conf/log4j.xml");
```

不过可能会出现`java.io.FileNotFoundException: F:\logs (拒绝访问。)`问题

#### 不过现在已经停止更新，官方建议使用log4j2了

