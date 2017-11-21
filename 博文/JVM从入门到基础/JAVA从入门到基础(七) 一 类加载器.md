JAVA从入门到基础(七) 一类加载器

### 一 Class装载验证流程

* 加载：装载类的第一个阶段会取得类的二进制流，之后转为方法区数据结构并在Java堆中生成对应的java.lang.Class对象。
* 链接

  1. 验证

     * 目的：保证Class流的格式是正确的
     * 文件格式的验证

       1. 是否以0xCAFEBABE开头

       2. 版本号是否合理
     * 元数据验证
       1. 是否有父类？
       2. 是否继承了final类？
       3. 非抽象类实现了所有的抽象方法！
     * 字节码验证 (很复杂)
       1. 运行检查
       2. 栈数据类型和操作码数据参数吻合
       3. 跳转指令指定到合理的位置
     * 符号引用验证
       1. 常量池中描述类是否存在
       2. 访问的方法或字段是否存在且有足够的权限

  2. 准备
     * –分配内存，并为类设置初始值 （方法区中）
       1. public static int v=1;
       2. 在准备阶段中，v会被设置为0
       3. 在初始化的<clinit>中才会被设置为1
       4. 对于static final类型，在准备阶段就会被赋上正确的值
       5. public static final  int v=1;
  3. 解析
     * 为了能真正使用，需要将符号引用替换为直接引用（指针或者地址偏移量引用对象一定在内存）
* 初始化
  * 执行类构造器<clinit>
    1. 对static变量 赋值语句
    2. static{}语句，会被执行。
  * 子类的<clinit>调用前保证父类的<clinit>被调用
  * <clinit>是线程安全的

### 二 什么是类装载器ClassLoader

* ClassLoader是一个抽象类。

* ClassLoader的实例将读入Java字节码将类装载到JVM中。

* ClassLoader可以定制，满足不同的字节码流获取方式，可以从网络中加载，也可以从文件加载。

* ClassLoader负责类装载过程中的加载阶段。

* public Class<?> loadClass(String name) throws ClassNotFoundException，载入并返回一个Class

* protected final Class<?> defineClass(byte[] b, int off, int len)，定义一个类，不公开调用。

* protected Class<?> findClass(String name) throws ClassNotFoundException，loadClass回调该方法，自定义ClassLoader的推荐做法。

* protected final Class<?> findLoadedClass(String name) 寻找已经加载的类。

  > ​	如图所示为ClassLoader结构图，在协同工作过程中，自底向上检查类是否加载，在寻找一个类时，首先在当前的ClassLoader下寻找是否已加载该类，如果自定义Class类无法找到该类，则不会加载，则将请求发送到父类，看父类是否有此类，如果有则返回，如果没有则再将请求发送到父类，直到BootStrap ClassLoader，之后进行自顶向下的对类进行加载。
  >
  > ​	在加载过程中存在双亲模式的问题：顶层ClassLoader，无法加载底层ClassLoader的类，为了解决此问题，可以使用Thread. setContextClassLoader()，一个上下文加载器，此加载器不完全是一个加载器，可以理解为是一个角色，用以解决顶层ClassLoader无法访问底层ClassLoader的类的问题，此加载器的基本思想是，在顶层ClassLoader中，传入底层ClassLoader的实例。从而突破双亲模式的局限性。
  >
  > - BootStrap ClassLoader （启动ClassLoader）
  > - Extension ClassLoader （扩展ClassLoader）
  > - App ClassLoader （应用ClassLoader/系统ClassLoader）
  > - Custom ClassLoader(自定义ClassLoader)
  > - 每个ClassLoader都有一个Parent作为父亲

  > ![ClassLoader结构图](http://img.blog.csdn.net/20171121232533322?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### 三 热替换

* 当一个class被替换后，系统无需重启，替换的类立即生效。使用ClassLoader可以实现热替换。