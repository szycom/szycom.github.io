## 编译HelloWorld程序

**编写一个简单的Welcome程序**

```java
/**
 * This program displays a greeting for the reader.
 * @version 
 * @author szycom
 */
public class Welcome {
	public static void main(String[] args) {
		String greeting = "Welcome to Java world!";
		System.out.println(greeting);
		for (int i = 0; i < greeting.length(); i++)
		{
			System.out.print("=");
		}
		System.out.println();
	}
}
```

**编译执行**

```html
javac Welcome.java        编译时要带上.java后缀 
java Welcome              执行时不能带后缀.class
```
![javacwelcome](/pics/javacwelcome.PNG)

> `javac`将`Welcome.java`编译成`Welcome.class`
> `java启动JVM虚拟机解释执行Welcome.class字节码文件`

**编译执行时注意如下几点**

* 执行命令时区分大小写，如果写成`javac welcome.java`则会报错
* 使用`javac`命令编译文件时需要带上`.java`后缀，使用`java`命令运行时不能带`.java或.class`后缀
* 当运行程序时提示找不到类文件，需要查看`CLASSPATH`变量是否设置正确
* 当类存在包时，需要在包所在目录执行程序

  如下程序，只有在**F:\JAVA\HelloJava**目录下执行`java welcome.Welcome`命令才能正确找到类`welcome.Welcome`

	```html
	F:\JAVA
	└─ HelloJava
	   └─ welcome
	      └─ Welcome.java
	```

	```java
	package welcome;

	/**
	* This program displays a greeting for the reader.
	* @version 
	* @author szycom
	*/
	public class Welcome {
		public static void main(String[] args) {
			String greeting = "Welcome to Java world!";
			System.out.println(greeting);
			for (int i = 0; i < greeting.length(); i++)
			{
				System.out.print("=");
			}
			System.out.println();
		}
	}
	```
