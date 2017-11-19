### JAVA 读取属性文件方式

### 一  代码完成

> ```java
> import org.apache.commons.lang3.StringUtils;
>
>
> import java.io.IOException;
> import java.io.InputStreamReader;
> import java.util.Properties;
>
> /**
>  * Created by OovEver on 2017/11/19.
>  */
> public class PropertiesUtil {
> //   声明Properties对象
>     private static Properties props;
>     static {
>         String fileName = "属性文件位置.properties";
>         props = new Properties();
>         try {
> //            读取配置文件
>             props.load(new InputStreamReader(PropertiesUtil.class.getClassLoader().getResourceAsStream(fileName),"UTF-8"));
>         } catch (IOException e) {
>             System.out.println("配置文件读取异常");
>         }
>     }
>
>     /***
>      *
>      * @param key 键值
>      * @return 返回获取结果
>      */
>     public static String getProperty(String key) {
>         String value = props.getProperty(key.trim());
> //        判断value是否为空，对于isBlank而言""， " "， "      "， null 都返回为空
>         if(StringUtils.isBlank(value)){
>             return null;
>         }
>         return value.trim();
>     }
>
>     /**
>      *
>      * @param key 键值
>      * @param defaultValue 如果未找到相应的value值，则以defaultValue代替
>      * @return  返回获取结果
>      */
>     public static String getProperty(String key,String defaultValue){
>
>         String value = props.getProperty(key.trim());
>         if(StringUtils.isBlank(value)){
>             value = defaultValue;
>         }
>         return value.trim();
>     }
> ```

### 二  StringUtils工具类

* 在上述代码中，用到了StringUtils 工具类，需要下载StringUtils JAR包，用于处理传来的字符串，

   1. 大家可以到 http://download.csdn.net/download/mupengfei6688/10125032 下载。 

   2. 或者从我的github上下载完整项目：https://github.com/oovever/javaUtil 。将下载后的资源包，导入到相应的开发工具即可使用。

   3. 或者自己写一个StringUtils工具类：

      ```java
      /**
       * Created by OovEver on 2017/11/19.
       */
      public class StringUtils {
          public static boolean isBlank(CharSequence cs) {
              int strLen;
              if(cs != null && (strLen = cs.length()) != 0) {
                  for(int i = 0; i < strLen; ++i) {
      //                看是否有空白字符
                      if(!Character.isWhitespace(cs.charAt(i))) {
                          return false;
                      }
                  }

                  return true;
              } else {
                  return true;
              }
          }
      }
      ```
> ### 附 ：所有java工具相关的代码，已放到github上，除了此工具类，还有其他工具类，欢迎大家补充，github地址 https://github.com/oovever/javaUtil
>

  