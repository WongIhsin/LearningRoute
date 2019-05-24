# Log4j2

---

log4j最简单，单独使用jar包就一个，只有输出功能，没有转接功能

log4j2单独使用时，jar包是`log4j-api-2.x.x.jar`和`log4j-core-2.x.x.jar`，有日志输出和转接功能

---

#### 依赖

```xml
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-core -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.11.1</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-api -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.11.1</version>
</dependency>
```

#### 配置

使用时需要使用`LogManager.getLogger()`

```java
Logger logger = LogManager.getLogger(LogManager.ROOT_LOGGER_NAME);
```

#### 配置文件

xml或者是json格式

位置是：log4j2默认会在classpath目录下寻找log4j2.xml、log4j.json、log4j.jsn等名称的文件，如果都没有找到，则会按默认配置输出，也就是输出到控制台。也可以对配置文件自定义位置（需要在web.xml中配置），一般放置在`src/main/rescources`根目录下

```java
public static void main(String[] args) throws IOException {
    File file = new File("D:/log4j2.xml");
    BufferedInputStream in = new BufferedInputStream(new FileInputStream(file));
    final ConfigurationSource source = new ConfigurationSource(in);
    Configurator.initialize(null, source);
    
    Logger logger = LogManager.getLogger("myLogger");
}
```

web工程方式

```xml
<context-param>
    <param-name>log4jConfiguration</param-name>
    <param-value>/WEB-INF/conf/log4j2.xml</param-value>
</context-param>

<listener>
    <listener-class>org.apache.logging.log4j.web.Log4jServletContextListener</listener-class>
</listener>
```

#### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <properties>
        <property name="LOG_HOME">D:/logs</property>
        <property name="FILE_NAME">mylog</property>
    </properties>
    
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
        </Console>
        
        <RollingRandomAccessFile name="RollingRandomAccessFile" fileName="${LOG_HOME}/${FILE_NAME}.log" filePattern="${LOG_HOME}/$${date:yyyy-MM}/${FILE_NAME}-%d{yyyy-MM-dd HH-mm}-%i.log">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n">
            </PatternLayout>
            <Policies>
                <TimeBasedTriggeringPolicy interval="1" />
                <SizeBasedTriggeringPolicy size="10MB" />
            </Policies>
            <DefaultRolloverStrategy max="20" />
        </RollingRandomAccessFile>
        
        <File name="FileAppender" fileName="D:/app.log">
            <PatternLayout pattern="%d{HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n" />
        </File>
        
        <Async name="AsyncAppender">
            <AppenderRef ref="RollingRandomAccessFile" />
        </Async>
    </Appenders>
    
    <Loggers>
        <Logger name="RollingRandomAccessFile" level="info" additivity="false">
            <AppenderRef ref="AsyncAppender" />
            <AppenderRef ref="Console" />
        </Logger>
        <!--
        <Root level="info">
            <AppenderRef ref="Console" />
        </Root>
		-->
    </Loggers>
</Configuration>
```

#### 配置文件详解

+ configuration：根节点，status属性为日志框架本身的日志级别；monitorInterval属性为每隔多少秒重新读取配置文件，可以在不重启应用的情况下修改配置
  + properties
  + Appenders：输出源。定义日志输出的地方
    + Console：控制台输出源是将日志打印到控制台上，开发的时候一般都会配置，以便调试
      + PatternLayout
    + File：文件输出源，用于将日志写入到指定的文件，需要配置输入到哪个位置
    + RollingRandomAccessFile：该输出源也是写入到文件，可以指定当文件到达一定大小时，另起一个文件继续写入，另起一个文件就涉及到新文件的命名规则，需要配置文件的命名规则
      + fileName：指定当前日志文件的位置和文件名称
      + filePattern：指定当发生Rolling时，文件的转移和重命名规则
      + Policies
        + TimeBasedTriggeringPolicy：interval="1"，结合filePattern中的文件重命名规则，最小时间粒度是分钟
    + Async：异步，需要通过AppenderRef来指定要对哪种输出源进行异步(一般用于配置RollingRandomAccessFile)
  + Loggers：日志器，分**根日志器Root**和**自定义日志器**
    + Logger：自定义时需要指定每个Logger的名称name（对于命名可以以包名作为日志的名字，不同的包配置不同的级别等），日志级别level，相加性additivity（是否继承下面配置的日志器），对于一般的日志器一般需要配置一个或多个输出源AppenderRef
      + AppenderRef
    + Root：根据日志名字获取不到指定的日志器时就使用Root作为默认的日志器
      + AppenderRef

additivity指定是否同时输出log到父类的appender，缺省为true