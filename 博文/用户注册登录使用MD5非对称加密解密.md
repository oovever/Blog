### 用户注册登录使用MD5非对称加密解密

### 一 简介

​	在我们日常开发中，有一些表字段，不适合明文存储，比如各个系统登录所使用的密码，这样做可以防止，一旦数据库遭遇黑客攻击，不至于导致密码等重要数据的外泄。由于现存的Md5字典中可以查询出一些常见的Md5密文，为了防止Md5密文被破解，在加密过程中，还需要对Md5密文进行加“盐”操作，从而使Md5密文无法被轻易破解。

### 二 具体实现

``` java
package util;

import java.security.MessageDigest;

/**
 * Created by OovEver on 2017/11/19.
 * MD5加密工具类
 */
public class MD5Util {
    private static String byteArrayToHexString(byte b[]) {
        StringBuffer resultSb = new StringBuffer();
        for (int i = 0; i < b.length; i++)
            resultSb.append(byteToHexString(b[i]));

        return resultSb.toString();
    }

    private static String byteToHexString(byte b) {
        int n = b;
        if (n < 0)
            n += 256;
        int d1 = n / 16;
        int d2 = n % 16;
        return hexDigits[d1] + hexDigits[d2];
    }

    /**
     * 返回大写MD5
     *
     * @param origin 要转化的md5字符串
     * @param charsetname 转化过程中使用的字符集
     * @return 返回大写MD5
     */
    private static String MD5Encode(String origin, String charsetname) {
        String resultString = null;
        try {
            resultString = new String(origin);
            MessageDigest md = MessageDigest.getInstance("MD5");
            if (charsetname == null || "".equals(charsetname))
                resultString = byteArrayToHexString(md.digest(resultString.getBytes()));
            else
                resultString = byteArrayToHexString(md.digest(resultString.getBytes(charsetname)));
        } catch (Exception exception) {
        }
        return resultString.toUpperCase();
    }

    /***
     * 调用这个函数进行md5非对称加密
     * @param origin 要加密的string字符串
     * @return 调用MD5Encode，返回大写MD5字符串
     */
    public static String MD5EncodeUtf8(String origin) {
//        加盐操作，防止通过MD5字典进行解密
        origin = origin + "geelysdafaqj23ou89ZXcj@#$@#$#@KJdjklj;D../dSF.,";
        return MD5Encode(origin, "utf-8");
    }


    private static final String hexDigits[] = {"0", "1", "2", "3", "4", "5",
            "6", "7", "8", "9", "a", "b", "c", "d", "e", "f"};
}

```

####  **<font color=red>附 ：所有java工具相关的代码，已放到github上，除了此工具类，还有其他工具类，欢迎大家补充，github地址:</font> https://github.com/oovever/javaUtil ** 