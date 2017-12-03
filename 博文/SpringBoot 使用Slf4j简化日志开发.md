### SpringBoot 使用Slf4j简化日志开发

### 一 添加Maven依赖

> ```java
>     <dependency>
>             <groupId>org.springframework.boot</groupId>
>             <artifactId>spring-boot-starter-data-jpa</artifactId>
>         </dependency>
>
>         <dependency>
>             <groupId>org.projectlombok</groupId>
>             <artifactId>lombok</artifactId>
>         </dependency>
> ```

### 二 添加注解

* 在类上添加@Slf4j注解

* 使用log输出日志

* 示例代码

  ```java
  package com.oovever;

  import lombok.extern.java.Log;
  import lombok.extern.slf4j.Slf4j;
  import org.apache.commons.logging.impl.SLF4JLog;
  import org.junit.Test;
  import org.junit.runner.RunWith;
  import org.springframework.boot.test.context.SpringBootTest;
  import org.springframework.test.context.junit4.SpringRunner;
  import sun.security.jgss.GSSToken;

  /**
   * @Author OovEver
   * @Date 2017/12/3 15:48
   */
  @RunWith(SpringRunner.class)
  @SpringBootTest
  @Slf4j
  public class LoggerTest {
      @Test
      public void test1() {
          log.debug("debug...");
          log.info("info...");
          log.error("error");
      }
  }

  ```

### 三 如果提示can not resolve log

* 安装lombok插件,idea->file->seetings->puugins->选择lombok Plugin即可

  ![lombok插件安装](http://img.blog.csdn.net/20171203160604089?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  ​