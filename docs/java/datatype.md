## 数值类型

> * 整数类型，**只有带符号的整数类型**，和C不一样没有无符号整数类型。
> * 浮点类型

下图列出了所有数值类型
![数值类型](/pics/number.PNG)

** 注意

> * JAVA中int类型固定占用4个字节，long类型固定占用8字节。
> 在C语言中int/long类型可能会因为处理器的不同而发生变化。
> * 浮点数[特殊存储方式][1]，在浮点数计算时可能会导致舍入误差，
> 对于精度要求比价高的场景可以使用[java.math.BigDecimal][2]

```java
public class Welcome {
	public static void main(String[] args) {
		System.out.println(0.9 - 0.8);     //输出结果为0.09999999999999998，而不是0.1
		System.out.println(0.9 - 0.7);     //输出0.2
		System.out.println(2 - 1.1);       //输出结果为0.8999999999999999，而不是0.9
		System.out.println(2 - 1.5);       //输出0.5
	}
}
System.out.println(2.0 - 1.1);  
```

[1]:https://docs.oracle.com/javase/specs/jls/se7/html/jls-4.html#jls-4.2.3 "Floating-Point Types, Formats, and Values"
[2]:https://docs.oracle.com/javase/8/docs/api/java/math/BigDecimal.html