# Log4j日志漏洞方案

## 一、Java Log场景

### 1、java常用日志框架

- sl4j（log门面）
- log4j/log4j2
- logback

### 2、使用方式

`保守如果项目中没有使用到但是引用了log4j那就彻底exclusion掉或者直接删除maven先关的依赖，如果使用到了log4j那就实行升级安全版本。`

- 单独使用log4j2

  > 这种需要升级log4j版本到修复过的版本，2.14.1已上。

- 单独使用logback

  > 无需处理log升级

- sl4j+log4j2

  > 这种需要升级log4j版本到修复过的版本，2.14.1已上。

- sl4j+logback（springboot默认方式，但是如果项目依然有依赖log4j需要具体场景具体处理。）

  >  无需处理log升级

```tex
明确各自框架业务log使用的以上场景。
```

### 3、SpringBoot日志处理方式

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-logging</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-api</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-core</artifactId>
        </exclusion>
        <exclusion>
            <groupId>org.apache.logging.log4j</groupId>
            <artifactId>log4j-jul</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-api</artifactId>
    <version>2.15.0</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.15.0</version>
</dependency>
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-jul</artifactId>
    <version>2.15.0</version>
</dependency>

```







