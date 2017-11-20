### JAVA 使用反射获得继承类

### 一代码完成

* 首先创建两个类，Parent.class与Children.class，Children类继承Parent类。

  > ```java
  > package util;
  >
  > /**
  >  * Children类
  >  * Created by OovEver on 2017/11/20.
  >  */
  > public class Children extends Parent {
  >     public static void main(String[] args) {
  >         Children children = new Children();
  >     }
  > }
  >
  > ```

  > ```java
  > package util;
  >
  > /**
  >  * Created by OovEver on 2017/11/20.
  >  * Parent类
  >  */
  > public class Parent {
  >     protected Class clazz;
  >     public Parent() {
  >         try {
  >             throw new Exception();
  >         } catch (Exception e) {
  > //            获得异常栈信息
  >             StackTraceElement stes[]= e.getStackTrace();
  > //            获得继承的子类名称,stes[0].getClassName为父类名称
  > //            如果想要获取继承children的类，即当前类的“孙子”类，则调用stes[2].getClassName,依此类推
  >             String serviceImpleClassName=   stes[1].getClassName();
  >             try {
  > //               通过反射获取类名
  >                 Class  serviceImplClazz= Class.forName(serviceImpleClassName);
  >                 String serviceImpleClassSimpleName = serviceImplClazz.getSimpleName();
  >                 System.out.println(serviceImpleClassSimpleName);
  > //                获取该类所在的包名
  >                 String pojoPackageName = serviceImplClazz.getPackage().getName();
  > //                获得该类的完整路径
  >                 String pojoFullName = pojoPackageName +"."+ serviceImpleClassSimpleName;
  >                 clazz=Class.forName(pojoFullName);
  >                 System.out.println(clazz);
  >             } catch (ClassNotFoundException e1) {
  >                 e1.printStackTrace();
  >             }
  >         }
  >     }
  > }
  >
  > ```

  ### 二 应用

  > 代码重构：在实际项目开发中，有许多公共的业务逻辑，如数据库常见的CURD操作，可以将其抽取出来，放到一个公共的父类处，让子类去继承这个父类，根据继承的子类不同，从而为不同的子类提供CURD操作。这样的实现方式，正是JAVA用委托实现代码重构的基本思路。

