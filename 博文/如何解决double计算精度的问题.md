###  如何解决double计算精度的问题

###  一  先看问题

首先看下面一段代码

> ```java
>  public static void main(String[] args) {
>         double a = 0.05;
>         double b = 0.01;
>         System.out.println(a + b);
>     }
> ```

> 看到这段代码，或许刚学到一天JAVA的同学都能将答案脱口而出，但是事实上的结果确是这样：

> ![代码运行结果](http://img.blog.csdn.net/20171119170753266?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

> 精度的丢失，导致得到的结果不是我们想要的，这种情况，如果出现在电商网站的支付、银行转账、刷卡支付等情况时，将对数据安全造成严重的损害，同时也极度影响用户的体验。例如现在我双十一买了两件产品，一件0.05元，一件0.01元（虽然这并不可能），我现在只有0.06元，正常的情况下，应该能成功购买，但是如果使用double直接进行想加减的话，将导致购买无法成功。
###  二  解决方式

> 使用BigDecimal对浮点数进行加减乘除运算，具体代码如下

> ```java
> import java.math.BigDecimal;
>
> /**
>  * Created by OovEver on 2017/11/19.
>  */
> public class BigDecimalUtil {
>     private BigDecimalUtil(){
>
>     }
>
>
>     public static BigDecimal add(double v1, double v2){
>         BigDecimal b1 = new BigDecimal(Double.toString(v1));
>         BigDecimal b2 = new BigDecimal(Double.toString(v2));
>         return b1.add(b2);
>     }
>
>     public static BigDecimal sub(double v1,double v2){
>         BigDecimal b1 = new BigDecimal(Double.toString(v1));
>         BigDecimal b2 = new BigDecimal(Double.toString(v2));
>         return b1.subtract(b2);
>     }
>
>
>     public static BigDecimal mul(double v1,double v2){
>         BigDecimal b1 = new BigDecimal(Double.toString(v1));
>         BigDecimal b2 = new BigDecimal(Double.toString(v2));
>         return b1.multiply(b2);
>     }
>
>     public static BigDecimal div(double v1,double v2){
>         BigDecimal b1 = new BigDecimal(Double.toString(v1));
>         BigDecimal b2 = new BigDecimal(Double.toString(v2));
>         return b1.divide(b2,2,BigDecimal.ROUND_HALF_UP);//四舍五入,保留2位小数
>
>         //除不尽的情况
>     }
>
> ```

> 使用BigDecimal 后加减乘除的结果如下所示：

> ![BigDecimal假发](http://img.blog.csdn.net/20171119172056561?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbXVwZW5nZmVpNjY4OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
>
> 