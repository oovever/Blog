### JAVA从入门到基础(三) 一 常用JVM配置参数

### 一 Treace跟踪参数

* -verbose:gc

* -XX:printGC

  1. 可以打印GC的简要信息

  > –[GC4790K->374K(15872K), 0.0001606 secs] (堆大小15872K，堆在GC之前，使用4790K，GC后为374K，花费时间0.0001606 secs)
  >
  > –[GC4790K->374K(15872K), 0.0001474 secs]
  >
  > –[GC4790K->374K(15872K), 0.0001563 secs]
  >
  > –[GC4790K->374K(15872K), 0.0001682 secs]

* -XX：+PrintGCDetails:

  1. 打印GC详细信息

  > – def new generation   total 13824K, used 11223K [0x27e80000(起始位置),0x28d80000(当前申请到的位置), 0x28d80000(最高边界，最高能申请到的位置))
  >
  > –  eden space 12288K,  91% used [0x27e80000, 0x28975f20, 0x28a80000)
  >
  > – from space 1536K,   0% used[0x28a80000, 0x28a80000, 0x28c00000)
  >
  > – to   space 1536K,   0% used [0x28c00000, 0x28c00000, 0x28d80000)
  >
  > – tenured generation   total 5120K, used 0K [0x28d80000,0x29280000, 0x34680000)
  >
  > –  the space 5120K,   0% used[0x28d80000, 0x28d80000, 0x28d80200, 0x29280000)
  >
  > – compacting perm gen  total 12288K, used 142K [0x34680000,0x35280000, 0x38680000)
  >
  > –  the space 12288K,   1% used[0x34680000, 0x346a3a90, 0x346a3c00, 0x35280000)
  >
  > –   ro space 10240K,  44% used [0x38680000, 0x38af73f0, 0x38af7400,0x39080000)
  >
  > –   rw space 12288K,  52% used [0x39080000, 0x396cdd28, 0x396cde00,0x39c80000)

* -XX:  +PrintGCTimeStamps 

  1. 打印GC发生的时间戳

  > –[GC[DefNew: 4416K->0K(4928K), 0.0001897 secs] 4790K->374K(15872K), 0.0002232 secs]
  >
  > –[Times: user=0.00 sys=0.00, real=0.00 secs] 

* -Xloggc:log/gc.log 

  1. 制定GC log的位置， 以文件方式输出，可以帮助开发人员分析问题
  2. 此命令，表示将日志输出在当前目录下的log/gc.log文件处

* -XX:+PrintHeapAtGC

  1. 每一次GC后都打印堆信息

     > Heapbefore GC invocations=0 (full 0):（<font color=red>**GC调用前**</font>）
     >
     >  def new generation   total 3072K, used 2752K [0x33c80000,0x33fd0000, 0x33fd0000)
     >
     >   eden space 2752K, 100% used[0x33c80000, 0x33f30000, 0x33f30000)
     >
     >   from space 320K,   0% used [0x33f30000, 0x33f30000, 0x33f80000)
     >
     >   to  space 320K,   0% used [0x33f80000,0x33f80000, 0x33fd0000)
     >
     >  tenured generation   total 6848K, used 0K [0x33fd0000,0x34680000, 0x34680000)
     >
     >    the space 6848K,   0% used [0x33fd0000, 0x33fd0000, 0x33fd0200,0x34680000)
     >
     >  compacting perm gen  total 12288K, used 143K [0x34680000,0x35280000, 0x38680000)
     >
     >    the space 12288K,   1% used [0x34680000, 0x346a3c58, 0x346a3e00,0x35280000)
     >
     > ​    ro space 10240K,  44% used [0x38680000, 0x38af73f0, 0x38af7400,0x39080000)
     >
     > ​    rw space 12288K,  52% used [0x39080000, 0x396cdd28, 0x396cde00,0x39c80000)
     >
     > [GC[DefNew:2752K->320K(3072K), 0.0014296 secs]2752K->377K(9920K), 0.0014604 secs]（<font color=red>**gc调用后**</font>）
     >
     > [Times: user=0.00 sys=0.00, real=0.00 secs]
     >
     > Heapafter GC invocations=1 (full 0):
     >
     >  def new generation   total 3072K, used 320K [0x33c80000,0x33fd0000, 0x33fd0000)
     >
     >   eden space 2752K,   0% used [0x33c80000, 0x33c80000, 0x33f30000)
     >
     >   from space 320K, 100% used [0x33f80000,0x33fd0000, 0x33fd0000)
     >
     >   to  space 320K,   0% used [0x33f30000,0x33f30000, 0x33f80000)
     >
     >  tenured generation   total 6848K, used 57K [0x33fd0000,0x34680000, 0x34680000)
     >
     >    the space 6848K,   0% used [0x33fd0000, 0x33fde458, 0x33fde600,0x34680000)
     >
     >  compacting perm gen  total 12288K, used 143K [0x34680000,0x35280000, 0x38680000)
     >
     >    the space 12288K,   1% used [0x34680000, 0x346a3c58, 0x346a3e00,0x35280000)
     >
     > ​    ro space 10240K,  44% used [0x38680000, 0x38af73f0, 0x38af7400,0x39080000)
     >
     > ​    rw space 12288K,  52% used [0x39080000, 0x396cdd28, 0x396cde00,0x39c80000)
     >
     > }

* -XX:+TraceClassLoading

  1. 监控类的加载

     > [Loaded java.lang.Object from shared objects file]
     >
     > [Loaded java.io.Serializable from shared objects file]
     >
     > [Loaded java.lang.Comparable from shared objects file]
     >
     > [Loaded java.lang.CharSequence from shared objects file]
     >
     > [Loaded java.lang.String from shared objects file]
     >
     > [Loaded java.lang.reflect.GenericDeclaration from shared objects file]
     >
     > [Loaded java.lang.reflect.Type from shared objects file]

* -XX:+PrintClassHistogram

  1. 打印类的直方图

  2. 在程序运行时，在控制台按下Ctrl+Break后，打印类的信息

     > ![打印的类的信息](http://img.blog.csdn.net/20171121164321276?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

     ><font color=red>**分别显示：序号、实例数量、总大小、类型**</font>



### 二 堆的分配参数

* -Xmx最大堆 –Xms最小堆
  1. 指定最大堆和最小堆，如运行运行-Xmx20m -Xms5m ，系统最大可以使用的空间20M，系统启动占用5M。
* -Xmn
  1. 设置新生代的大小
* -XX:NewRatio
  1. 设置新生代打下
  2. 与-Xmn区别，-Xmn用于设置一个绝对值，是多少就是多少，NewRatio用于设置比例，如果后面参数为4，则新生代：老年代=1:4，即年轻代占堆的20%。
* n-XX:SurvivorRatio
  1. 设置两个Survivor区和eden的比
  2. 8表示 两个Survivor :eden=2:8，即一个Survivor占年轻代的1/10
* XX:+HeapDumpOnOutOfMemoryError
  1. OOM时导出堆到文件
* -XX:+HeapDumpPath
  1. 导出OOM的路径
* -XX:OnOutOfMemoryError
  1. 在OOM时，执行一个脚本
  2. "-XX:OnOutOfMemoryError=D:/tools/jdk1.7_40/bin/printstack.bat %p“,系统dump后调用printstack.bat
  3. 当程序OOM时，在D:/a.txt中将会生成线程的dump
  4. 可以在OOM时，发送邮件，甚至是重启程序
* n根据实际事情调整新生代和幸存代的大小
* n官方推荐新生代占堆的3/8
* n幸存代占新生代的1/10
* n在OOM（out of memory）时，记得Dump出堆，确保可以排查现场问题

### 三 永久区的分配参数

 * -XX:PermSize  -XX:MaxPermSize
   1. 设置永久区的初始空间和最大空间
   2. 他们表示，一个系统可以容纳多少个类型

### 四 栈的大小分配

* -Xss
  1. 分配栈时通常只有几百K，线程*栈空间=系统所有线程所占空间
  2. 决定了函数调用的深度，栈空间太小可能出现栈的溢出
  3. 每个线程都有独立的栈空间
  4. 局部变量、参数 分配在栈上