# 日志接口slf4j

slf4j 全称为**Simple Logging Facade for Java**，java简单日志门面

slf4j是对所有日志框架指定的一种规范、标准、接口，并不是一个框架的具体的实现，因为接口并不能独立使用，需要和具体的日志框架实现配合使用

**日志实现**：log4j是apache实现的一个开源日志组件；logback同样是由log4j的作者设计完成的，拥有更好的特性，用来取代log4j的一个日志框架，是slf4j的原生实现；log4j2是log4j 1.x和logback的改进版，据说采用了一些新技术，无锁异步等，使得日志的吞吐量、性能比log4j 1.x提高10倍，并解决了一些死锁的bug，而且配置更加简单灵活

#### 为什么是日志接口

slf4j定义了一套日志接口，项目中使用的日志框架是logback，开发中调用的所有接口都是slf4j的，不直接使用logback，调用是自己的工程调用slf4j的接口，slf4j的接口去调用logback的实现，可以看到整个过程应用程序并没有直接使用logback

当项目需要更换更加优秀的日志框架，如log4j2只需要引入log4j2的jar和log4j2对应的配置文件即可，完全不用更改Java代码中的日志相关的代码，也不用修改日志相关的类的导入的包（`import org.slf4j.Logger; import org.slf4j.LoggerFactory;`）

使用日志接口便于更换为其他日志框架，log4j、logback、log4j2都是一种日志具体实现框架，所以既可以单独使用也可以结合slf4j一起搭配使用

---

#### 依赖

```xml
<!-- log4j2核心包 -->
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

<!-- 用于与slf4j保持桥接 -->
<!-- https://mvnrepository.com/artifact/org.apache.logging.log4j/log4j-slf4j-impl -->
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-slf4j-impl</artifactId>
    <version>2.11.1</version>
</dependency>
<!-- slf4j核心包 -->
<!-- https://mvnrepository.com/artifact/org.slf4j/slf4j-api -->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
```

需要注意

```java
//需要引入import org.slf4j.Logger;
//需要引入import org.slf4j.LoggerFactory;
//而不是import org.apache.logging.log4j.LogManager;
//而不是import org.apache.logging.log4j.Logger;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class Log4j2Test {
    private final static Logger logger = LoggerFactory.getLogger(Log4j2Test.class);
}
```

