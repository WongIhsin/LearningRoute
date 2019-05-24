# Junit详解

一个用于单元测试的小框架，基于xUnit架构的一个实现。xUnit是一套基于测试驱动开发的测试框架；子集有PythonUnit/CppUnit/JUnit等

#### 依赖

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.11</version>
  <scope>test</scope>
</dependency>
```

#### Junit中的基本注解

+ @BeforeClass：表示在类中的任意public static void方法执行之前执行，必须为静态static方法。当测试类被加载后就会执行它，在内存中只会存在一份实例，比较适合加载测试文件

+ @AfterClass：表示在类中的任意public static void方法执行之后执行，必须为静态static方法。通常用来对资源的清理

+ @Before：表示在任意使用@Test注解标注的public void方法执行之前执行

+ @After：表示在任意使用@Test注解标注的public void方法之后执行

+ @Test：使用该注解标注的public void方法会表示为一个测试方法

+ @Ignore：测试运行器忽略该方法，也可以加上说明信息

  ```java
  @Ignore
  @Test
  public void shouldAnswerWithTrue4()
  {
      assertTrue( true );
      System.out.println("Test");
  }
  
  @Ignore("message")
  @Test
  public void shouldAnswerWithTrue4()
  {
      assertTrue( true );
      System.out.println("Test");
  }
  ```

+ @RunWith：可以更改测试运行器，自定义的测试运行器需要继承`org.junit.runner.Runner`

#### 项目最佳实践src/test

#### @Test属性

`@Test(expected = XX.class)`：捕获异常

`@Test(timeout = 毫秒)`：设置超时

```java
@Test(expected = ArithmeticException.class)
public void shouldAnswerWithTrue2() {
    System.out.println("A");
    throw new ArithmeticException();
}

@Test(timeout = 9000)
public void shouldAnswerWithTrue3() {
    System.out.println("A");
    try {
        Thread.sleep(10000);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```

`assertTrue`是静态的类方法，使用了静态导入

```java
import static org.junit.Assert.assertTrue;

@Test
public void shouldAnswerWithTrue()
{
    assertTrue( true );
    System.out.println("Test");
}
```

---

#### 测试套件

组织测试类一起运行：需要使用一个public空类作为测试套件的入口类，这个类不包含其他方法，使用@RunWith注解更改测试运行器`@RunWith(Suite.class)`；将要测试的类作为数组传入到`@Suite.SuiteClasses({A.class, B.class, ...})`

```java
import org.junit.runner.RunWith;
import org.junit.runners.Suite;

@RunWith(Suite.class)
@Suite.SuiteClasses({AppTest.class, ...})
public class SuiteTest {
}
```

---

#### 参数化设置

```java
import org.junit.runner.RunWith;
import org.junit.runners.Parameterized;

@RunWith(Parameterized.class)
public class ParameterTest {
    int expected = 0;
    int input1 = 0;
    int input2 = 0;

    @Parameterized.Parameters
    public static Collection<Object[]> t() {
        return Arrays.asList(new Object[][] {
                {3,1,2},
                {4,2,2}
        });
    }

    public ParameterTest(int expected, int input1, int input2) {
        this.expected = expected;
        this.input1 = input1;
        this.input2 = input2;
    }

    @Test
    public void testAdd() {
        assertEquals(expected, input2 + input1);
    }
}
```

+ 首先需要更默认的测试运行器为`@RunWith(Parameterized.class)`
+ 声明变量来存放预期值和结果值
+ 声明一个返回值为Collection的公共静态方法，并使用`@Parameterized.Parameters`修饰
+ 为测试类声明一个带有参数的公共构造函数，并在其中为之声明变量赋值

---

#### 框架整合测试

