### JVM从入门到基础(二) 一JVM运行机制

### 一 JVM启动流程

> 1. 使用JAVA命令+启动类，启动类中包含Main方法。
> 2. 在使用java命令启动类后，首先会进行装载配置。
> 3. 在装载配置过程中，会根据当前路径和系统版本，寻找JVM.cfg文件。
> 4. 根据配置寻找JVM.dll文件，JVM.dll为JAVA虚拟机的主要实现
> 5. 使用获得的dll初始化虚拟机，获得JNIEnv接口（提供大量和JVM交互的方法）
> 6. 找到Main方法并运行
>
> ![JVM启动流程](http://img.blog.csdn.net/20171120223122655?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>

###  二 JVM基本结构

> ![JVM基本结构](http://img.blog.csdn.net/20171120223441613?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
> Class文件通过类加载器子系统将Class文件加载到内存空间。
>
> * 方法区
>   1. 保存装载的类信息
>      * 类型的常量池
>      * 字段，方法信息
>      * 方法字节码
>   2. 通常和永久区（Perm）（相对静止相对稳定的数据）关联在一起
>
> * JAVA 堆
>
>   1. 和程序开发密切相关（New的对象都在堆中）
>   2. 应用系统对象都保存在Java堆中
>   3. 所有线程共享Java堆（分配一个变量后，所有变量都可以使用）
>   4. 对分代GC来说，堆也是分代的
>   5. GC的主要工作区间
>
> * JAVA 栈
>
>   1. 线程私有
>
>   2. 栈由一系列帧组成（因此Java栈也叫做帧栈）
>
>   3. 帧保存一个方法的局部变量、操作数栈、常量池指针
>
>   4. 每一次方法调用创建一个帧，并压栈
>
>   5. JAVA没有寄存器，所有参数传递使用操作数栈
>
>      ```java
>      public static int add(int a,int b){
>      int c=0;
>      c=a+b;
>      return c;
>      }
>
>      ```
>
>       0:   iconst_0 // 0压栈
>
>       1:   istore_2// 弹出int，存放于局部变量2(即c)
>
>       2:   iload_0  //把局部变量0压栈
>
>       3:  iload_1 // 局部变量1压栈
>
>       4:   iadd      //弹出2个变量，求和，结果压栈
>
>       5:   istore_2//弹出结果，放于局部变量2
>
>       6:   iload_2  //局部变量2压栈
>
>       7:   ireturn  //返回
>
> * 栈、堆、方法区交互
>
>   ```java
>   public   class  AppMain     
>    //运行时, jvm 把appmain的信息都放入方法区 
>   { 
>     public   static   void  main(String[] args)  
>   //main 方法本身放入方法区。 
>   { Sample test1 = new  Sample( " 测试1 " );  
>    //test1是引用，所以放到栈区里， Sample是自定义对象应该放到堆里面 
>    Sample test2 = new  Sample( " 测试2 " ); 
>    test1.printName();
>    test2.printName();
>   } 
>
>   ```
>
>   ``` java
>   public   class  Sample       
>    //运行时, jvm 把appmain的信息都放入方法区 
>   { private  name;     
>    //new Sample实例后， name 引用放入栈区里，  name 对象放入堆里 
>    public  Sample(String name) { this .name = name; } 
>    //print方法本身放入 方法区里。
>    public   void  printName()   
>    { 
>      System.out.println(name);
>    } 
>   }
>
>   ```
>
>   ​
>
>   ![栈、堆、方法区交互](http://img.blog.csdn.net/20171120225503854?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
> * PC 寄存器
>   1. 每个线程拥有一个PC寄存器
>   2. 在线程创建时创建
>   3. 指向下一条指令的地址
>   4. 执行本地方法时,PC的值为undefined
>
> * JAVA内存模型
>
>   1. 每一个线程有一个工作内存和主存独立
>
>   2. 工作内存存放主存中变量的值的拷贝
>
>      ![JAVA内存模型](http://img.blog.csdn.net/20171120230015187?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
>      * 当数据从主内存复制到工作存储时，必须出现两个动作：第一，由主内存执行的读（read）操作；第二，由工作内存执行的相应的load操作；当数据从工作内存拷贝到主内存时，也出现两个操作：第一个，由工作内存执行的存储（store）操作；第二，由主内存执行的相应的写（write）操作。
>
>      * 每一个操作都是原子的，即执行期间不会被中断
>
>      * 对于普通变量，一个线程中更新的值，不能马上反应在其他变量中，如果需要在其他线程中立即可见，需要使用 volatile 关键字（性能比锁好，线程不安全，不能代替锁）。
>
>        ![内存模型描述](http://img.blog.csdn.net/20171120230358733?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
>   3. 可见性：一个线程修改了变量，其他线程可以立即知道。保持可见性的方法有以下几种
>
>      1. volatile
>      2. synchronized （unlock之前，写变量值回主存）
>      3. final(一旦初始化完成，其他线程就可见)
>
>   4. 有序性：–在本线程内，操作都是有序的，–在线程外观察，操作都是无序的。（指令重排 或 主存与工作内存同步延时）。保证有序性的方式，加synchronized 。指令重排的基本原则：
>
>      1. 程序顺序原则：一个线程内保证语义的串行性
>      2. volatile规则：volatile变量的写，先发生于读
>      3. 锁规则：解锁(unlock)必然发生在随后的加锁(lock)前
>      4. 传递性：A先于B，B先于C 那么A必然先于C
>      5. 线程的start方法先于它的每一个动作
>      6. 线程的所有操作先于线程的终结（Thread.join()）
>      7. 线程的中断（interrupt()）先于被中断线程的代码
>      8. 对象的构造函数执行结束先于finalize()方法

​	