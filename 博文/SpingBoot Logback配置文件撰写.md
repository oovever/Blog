### SpingBoot Logback配置文件撰写

### 一 简单方式--直接在application.yml中书写

* 示例：

  ```
  logging:
    pattern:
  #日志的输出格式为时间---日志信息加换行
      console: "%d - %msg%n"
  #path: /var/log/tomcat/ 表示将文件输出到var/log/tomcat/下的log文件下
    #path: /var/log/tomcat/
  #将文件输出到/var/log/tomcat/下的sell.log文件下
    file: /var/log/tomcat/sell.log
  #输出 com.imooc.LoggerTest下的debug的日志信息
    level:
     com.imooc.LoggerTest: debug
  ```

### 二 使用复杂模式----logback.xml

* 配置:

  ```
  <?xml version="1.0" encoding="UTF-8" ?>
  <!--根节点-->
  <configuration>
  <!--配置项-->
      <appender name="consoleLog" class="ch.qos.logback.core.ConsoleAppender">
          <!--展现形式-->
          <layout class="ch.qos.logback.classic.PatternLayout">
              <pattern>
                  <!--以日期+日志信息+换行-->
                  %d - %msg%n
              </pattern>
          </layout>
      </appender>
  <!--配置文件，滚动文件输出，每天输出一个文件-->
      <appender name="fileInfoLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
          <!--根据范围进行过滤-->
          <filter class="ch.qos.logback.classic.filter.LevelFilter">
              <!--匹配就禁止操作-->
              <level>ERROR</level>
              <onMatch>DENY</onMatch>
              <!--否则接受处理操作-->
              <onMismatch>ACCEPT</onMismatch>
          </filter>
          <encoder>
              <pattern>
                  %msg%n
              </pattern>
          </encoder>
          <!--滚动策略，按照时间滚动，每天一个文件-->
          <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
              <!--路径-->
              <fileNamePattern>/var/log/tomcat/sell/info.%d.log</fileNamePattern>
          </rollingPolicy>
      </appender>

  <!--错误文件输出的位置-->
      <appender name="fileErrorLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
          <!--只输出error信息-->
          <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
              <level>ERROR</level>
          </filter>
          <encoder>
              <pattern>
                  %msg%n
              </pattern>
          </encoder>
          <!--滚动策略-->
          <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
              <!--路径-->
              <fileNamePattern>/var/log/tomcat/sell/error.%d.log</fileNamePattern>
          </rollingPolicy>
      </appender>
  <!--使用配置项-->
      <root level="info">
          <appender-ref ref="consoleLog" />
          <appender-ref ref="fileInfoLog" />
          <appender-ref ref="fileErrorLog" />
      </root>

  </configuration>
  ```

  ​

