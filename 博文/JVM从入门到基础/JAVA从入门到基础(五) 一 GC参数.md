### JAVA从入门到基础(五) 一 GC参数

### 一 串行回收器

* 作为JVM自带的垃圾回收器的一种，是一种最古老，最稳定的垃圾回收器。

* 效率最高

* 因为串行回收器在使用时，只使用一个线程进行回收，在多核计算机的情况下，无法充分发挥计算机性能，造成计算机资源浪费，所以可能会产生较长的停顿。

* -XX:+UseSerialGC 启用串行回收器，在使用串行回收器后，新生代、老年代使用串行回收，其中在新生代复制算法在老年代标记-压缩。

  > 在串行回收的过程中，初始应用进程可能有多个，但是一旦回收开始， 应用程序线程全部暂停，用GC线程接替应用线程，在GC完成后，恢复应用线程的运行。

  > ![串行回收器运行过程](http://img.blog.csdn.net/20171121213802835?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 二 并行收集器

###     

* ParNew
  * 使用XX:+UseParNewGC启动执行，在并行收集器中新生代使用并行回收，老年代使用串行回收，所以并行收集器只影响新生代的收集，不影响老年代的收集。Serial收集器新生代的并行版本。在回收过程中使用复制算法复制算法，可以在多线程下执行，所以可以在多核计算机下执行情况较好。

  * -XX:ParallelGCThreads 可以限制线程数量

    > 在并行回收的过程中，初始应用进程可能有多个，但是一旦回收开始， 应用程序线程全部暂停，多个GC线程接替应用线程，并行的执行垃圾回收操作，在GC完成后，恢复应用线程的运行。但是多线程不一定比单线程快，要分情况而定，在单核CPU上，串行收集器性能较好。

    > ![并行回收器](http://img.blog.csdn.net/20171121214541533?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  * Parallel收集器

    * 类似ParNew,与ParNew不同的是Parrallel更加关注吞吐量，在Parallel收集器，对新生代使用复制算法， 对老年代使用标记压缩算法。

    * XX:+UseParallelGC 启用并行收集器，启用之后，新生代使用Parallel收集器，老年代使用串行收集器。

    * XX:+UseParallelOldGC启用并行收集器，新生代使用Parallel收集器，同时老年代也使用并行收集器。

      > 在并行回收的过程中，初始应用进程可能有多个，但是一旦回收开始， 应用程序线程全部暂停，多个GC线程接替应用线程，并行的执行垃圾回收操作，在GC完成后，恢复应用线程的运行。但是多线程不一定比单线程快，要分情况而定，在单核CPU上，串行收集器性能较好。

      > ![并行回收器](http://img.blog.csdn.net/20171121214541533?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  * -XX:MaxGCPauseMills，最大停顿时间，单位毫秒，GC尽力保证回收时间不超过设定值

  * -XX:GCTimeRatio，0-100的取值范围，垃圾收集时间占总时间的比，默认99，即最大允许1%时间做GC

  * 上述两个参数是矛盾的。因为停顿时间和吞吐量不可能同时调优。

### 三 CMS收集器

* CMS收集器是Concurrent Mark Sweep 并发标记清除，与用户线程一起执行。使用标记-清除算法，并发阶段会降低吞吐量，在CMS收集器执行过程中，老年代使用CMS收集器，新生代使用ParNew收集器。

* –-XX:+UseConcMarkSweepGC 启动CMS收集器

* CMS的主要工作

  1. 初始标记，由根对象可以直接关联到的对象要做初始标记，此标记速度较快。在标记过程中需要产生全局停顿。

  2. 并发标记（和用户线程一起），用户线程一边执行CMS一边进行标记，全部对象都需要被标记。

  3. 重新标记，由于并发标记时，用户线程依然运行，因此在正式清理前，再做修正。

  4. 并发清除（和用户线程一起），用户线程一边执行CMS一边进行清理，基于标记结果，直接清理对象。

     > 在CMS手机过程中，首先多个应用程序执行，当需要执行垃圾回收时，应用程序暂停，进行CMS单线程的初始标记，由于初始标记只标记根对象的直接可达对象，所以标记时间较短，初始标记完成之后，应用程序继续执行，此时进行并发标记，并发标记过程中扫描所有对象，并对对象进行标记，由于在标记过程中，有应用程序执行，所以会有新的垃圾产生，故在并发标记后暂停应用程序线程的执行，进行重新标记，进行标记修正，由于重新标记作为并发标记的补充，所以花费时间较短。重新标记后，所有垃圾对象可寻出，此时应用程序线程开启，并发可执行清理工作，对垃圾对象进行清理。

     > ![CMS收集器](http://img.blog.csdn.net/20171121220542023?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

* CMS收集器的特点

  1. 可以尽可能的降低停顿。

  2. 会影响系统整体吞吐量和性能，比如，在用户线程运行过程中，分一半CPU去做GC，系统性能在GC阶段，反应速度就下降一半。

  3. 清理不彻底，因为在清理阶段，用户线程还在运行，会产生新的垃圾，无法清理。

  4. 因为和用户线程一起运行，不能在空间快满时再清理，-XX:CMSInitiatingOccupancyFraction设置触发GC的阈值

     如果不幸内存预留空间不够，就会引起concurrent mode failure。

  5. 由于CMS使用的是标记-清除算法，所以在执行CMS收集之后，在内存中可能产生大量的碎片，而标记压缩算法由于其特点则不会产生碎片。所以为了防止使用标记清除算法后产生的碎片，需要在标记清除算法之后，执行一次标记压缩算法。标记清除可以并发执行，标记压缩只能串行执行。下面1,2为两个CMS命令，去整理内存碎片。在这些命令执行时都会产生停顿。

     1. -XX:+ UseCMSCompactAtFullCollection Full GC后，进行一次整理，整理过程是独占的，会引起停顿时间变长。
     2. -XX:+CMSFullGCsBeforeCompaction ，设置进行几次Full GC后，进行一次碎片整理
     3. -XX:ParallelCMSThreads,设定CMS的线程数量（一般为可用CPU数量）

     > ![标记清除与标记压缩](http://img.blog.csdn.net/20171121221832881?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

  6. CMS只是减少停顿，并没有从根本上解决停顿问题。






