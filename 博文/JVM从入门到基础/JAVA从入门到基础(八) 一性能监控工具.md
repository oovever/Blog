### JAVA从入门到基础(八) 一性能监控工具

### 一系统性能监控--linux

* uptime，此命令会显示以下内容
  1. 系统时间
  2. 系统运行时间，表示从开机到现在为止的运行时间，如图表示开机1分钟。
  3. 连接数，每一个终端算一个连接，如图表示一个User。
  4. 分别为1,5,15分钟内的系统平均负载，运行队列中的平均进程数，如图表示1,5,15平均进程数分别为3.5,1.06,0.37。

> ![uptime](http://img.blog.csdn.net/20171122151815012?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* top 命令

  1. 第一行与uptime相似。
  2. 可以显示CPU与内存情况。
  3. 显示表格内所有进程情况，显示CPU与内存占用率。

  ![top命令](http://img.blog.csdn.net/20171122152054056?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* vmstat，–可以统计系统的CPU，内存，swap，io等情况

  ![vmstat命令](http://img.blog.csdn.net/20171122152329540?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* pidstat ，细致观察进程

  1. –需要安装 sudo apt-get install sysstat

  2. 可以监控CPU

  3. 可以监控IO

  4. 可以监控内存

  5. 使用方式 pidstat -p 指定进程–u 监控CPU 每秒采样 采样次数，如pidstat -p 2962 -u 1 3则表示对2962进程CPU进行监控，每秒采样一次，共采样3次。

  6. pidstat  -t 显示进程

### 二 系统性能监控 - windows

* 任务管理器

  > ![任务管理器](http://img.blog.csdn.net/20171122153523480?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  >   ![任务管理器](http://img.blog.csdn.net/20171122153552988?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* Perfmon,–Windows自带多功能性能监控工具,在命令行输入Perfmon即可打开页面

  > ![Perfomn](http://img.blog.csdn.net/20171122153655501?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  >   ![Perfmon](http://img.blog.csdn.net/20171122153924415?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  > ![这里写图片描述](http://img.blog.csdn.net/20171122153945142?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  > ![这里写图片描述](http://img.blog.csdn.net/20171122154002278?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* Process Explorer

  > ![Process Explorer](http://img.blog.csdn.net/20171122154133118?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* pslist

  1. 命令行工具


  2. 可用于自动化数据收集

  3. 显示java程序的运行情况

  ### 三 JAVA自带的工具

  * jps

    1. 列出java进程，类似于ps命令
    2. 参数-q可以指定jps只输出进程ID ，不输出类的短名称
    3. 参数-m可以用于输出传递给Java进程（主函数）的参数
    4. 参数-l可以用于输出主函数的完整路径
    5. 参数-v可以显示传递给JVM的参数

  * jinfo

    1. 可以用来查看正在运行的Java应用程序的扩展参数，甚至支持在运行时，修改部分参数

    2. flag <name>：打印指定JVM的参数值

    3. flag [+|-]<name>：设置指定JVM参数的布尔值

    4. flag <name>=<value>：设置指定JVM参数的值

       1. > 显示了新生代对象晋升到老年代对象的最大年龄，jinfo-flag MaxTenuringThreshold 2972-XX:MaxTenuringThreshold=15

       2. > 显示是否打印GC详细信息jinfo-flag PrintGCDetails  2972-XX:-PrintGCDetails

       3. > 运行时修改参数，控制是否输出GC日志jinfo-flag PrintGCDetails 2972-XX:-PrintGCDetails
          >
          > jinfo-flag +PrintGCDetails 2972 jinfo-flag PrintGCDetails 2972 -XX:+PrintGCDetails

    * jmap
      1. 生成Java应用程序的堆快照和对象的统计信息
      2. jmap -histo 2972 >c:\s.txt
    * Dump堆
      1. –jmap -dump:format=b,file=c:\heap.hprof 2972
    * jstack
      1. 打印线程dump
      2. -l打印锁信息
      3. m 打印java和native的帧信息
      4. -F 强制dump，当jstack没有响应时使用
    * JConsole
      1. 图形化监控工具
      2. 可以查看Java应用程序的运行概况，监控堆信息、永久区使用情况、类加载情况等
    * Visual VM
      1. Visual VM是一个功能强大的多合一故障诊断和性能监控的可视化工具

  ​