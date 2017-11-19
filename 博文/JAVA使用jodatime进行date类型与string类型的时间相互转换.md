###　JAVA使用jodatime进行date类型与string类型的时间相互转换

###  一 jodatime简介

> Joda-Time 令时间和日期值变得易于管理、操作和理解。事实上，易于使用是 Joda 的主要设计目标。其他目标包括可扩展性、完整的特性集以及对多种日历系统的支持。并且 Joda 与 JDK 是百分之百可互操作的，因此您无需替换所有 Java 代码，只需要替换执行日期/时间计算的那部分代码。
>
> 详细介绍，参考：https://www.ibm.com/developerworks/cn/java/j-jodatime.html

### 二 具体实现

```java
public class DateTimeUtil {
    public static final String STANDARD_FORMAT = "yyyy-MM-dd HH:mm:ss";

    /***
     * 将string字符串转化为Date类型的字符串
     * @param dateTimeStr 需要转化的string类型的字符串
     * @param formatStr 转化规则
     * @return 返回转化后的Date类型的时间
     */
    public static Date strToDate(String dateTimeStr, String formatStr){
        DateTimeFormatter dateTimeFormatter = DateTimeFormat.forPattern(formatStr);
        DateTime dateTime = dateTimeFormatter.parseDateTime(dateTimeStr);
        return dateTime.toDate();
    }

    /***
     * 将date类型的时间转化为string类型
     * @param date 需要转化的date类型的时间
     * @param formatStr 转化规则
     * @return 返回转化后的string类型的时间
     */
    public static String dateToStr(Date date,String formatStr){
        if(date == null){
            return StringUtils.EMPTY;
        }
        DateTime dateTime = new DateTime(date);
        return dateTime.toString(formatStr);
    }

    /***
     * 将string字符串转化为Date类型的字符串
     * @param dateTimeStr 需要转化的string类型的时间
     * @return 返回转化后的Date类型的时间
     */
    public static Date strToDate(String dateTimeStr){
        DateTimeFormatter dateTimeFormatter = DateTimeFormat.forPattern(STANDARD_FORMAT);
        DateTime dateTime = dateTimeFormatter.parseDateTime(dateTimeStr);
        return dateTime.toDate();
    }

    /***
     * 将date类型的时间转化为string类型
     * @param date 需要转化的date类型的时间
     * @return 返回转化后的string类型的时间
     */
    public static String dateToStr(Date date){
        if(date == null){
            return StringUtils.EMPTY;
        }
        DateTime dateTime = new DateTime(date);
        return dateTime.toString(STANDARD_FORMAT);
    }
    public static void main(String[] args) {
        System.out.println(DateTimeUtil.dateToStr(new Date(),"yyyy-MM-dd HH:mm:ss"));
        System.out.println(DateTimeUtil.strToDate("2010-01-01 11:11:11","yyyy-MM-dd HH:mm:ss"));

    }
}

```

### 三 jodatime jar包下载

在上述代码中，用到了StringUtils 工具类，需要下载StringUtils JAR包，用于处理传来的字符串，

1. 大家可以到 http://download.csdn.net/download/mupengfei6688/10125032 下载。 

2. 或者从我的github上下载完整项目：https://github.com/oovever/javaUtil 。将下载后的资源包，导入到相应的开发工具即可使用。

####  **<font color=red>附 ：所有java工具相关的代码，已放到github上，除了此工具类，还有其他工具类，欢迎大家补充，github地址:</font> https://github.com/oovever/javaUtil ** 


