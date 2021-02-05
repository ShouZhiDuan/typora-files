# JVM系列篇

## 一、基础篇

### 1、什么是JVM

```
个人简单理解：jvm是为了解决java程序能够跨平台运行的一个强大的软件。他可以为不同的平台Linux、Windows等，编译以及解释(翻译)源文件。从而实现一处编写多次运行，也就实现跨平台的意义。
```

![image-20210107104447851](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210107104447851.png)



### 2、JDK、JRE、JVM什么关系

- [官网解释](https://docs.oracle.com/javase/8/docs/index.html)

  > 如下图所示

![image-20210107105029787](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210107105029787.png)

- JDK 

  ```tex
  从上图可知JDK范围包含JRE以及JVM。JDK与JRE和JVM相比多出了一些java运行环境以外的常用工具。
  比如说：java jar javac javap等。
  java -jar test.jar 有用到java jar工具。
  javac Test.java 编译源文件-> Test.class.
  javap Test.class 反编译查看编译后的明文指令。 -> Test.txt.
  ```

- JRE

  ```tex
  提供了一些程序运行的工具类。常见的有
  Collectios 集合工具
  Math       数学工具
  JDBC       数据库工具
  Reflection 反射工具
  ```

- JVM

  ```tex
  负责跨平台编译、解释工作。实现java程序跨平台运行的功能。
  ```

  

### 3、JVM工作原理

- 简单示意图

![image-20210107113639703](C:\Users\dev\AppData\Roaming\Typora\typora-user-images\image-20210107113639703.png)

```
(1)源码到类文件 
(2)类文件到JVM 
(3)JVM各种折腾[内部结构、执行方式、垃圾回收、本地调用等]
```

- 源码到类文件

  ```java
  package com.example.dsz.jvm.do_test;
  
  /**
   * @Auther: ShouZhi@Duan
   * @Date: 2021/1/7 11:20
   * @Description:
   */
  public class JvmTestDo {
  
      private String privateName = "privateName";
      public int intB = 1;
      public static int intC = 2;
      private static String staticStringNul;
      public static String staticStringName = "staticName";
      private static JvmTestDo jvmTestDo = new JvmTestDo();
      public final int finalIntC = 1;
      public final static int finalIntD = 1;
      public final String finalString = "finalString";
      public final static String finalStaticString = "finalStaticString";
  
      static void testDo(){
          int c;
          c = intC + 1;
          System.out.println(c);
      }
  
      public static void main(String[] args) {
            testDo();
      }
  
  }
  ```

如何由JvmTestDo.java->JvmTestDo.class文件，执行javac  -g:vars  JvmTestDo.java，会在当前目录生成JvmTestDo.class文件。

























