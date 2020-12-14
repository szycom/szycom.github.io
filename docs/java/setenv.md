## PATH
`PATH`变量指定可执行程序的搜索路径，执行命令时系统会到PATH指定的路径中查找可执行文件，如果找不到则提示错误信息。

当然执行命令时也可以输入完整的命令，但是这样操作比较麻烦。

**举例: 编译Hello.java程序**

```html
F:\JAVA
└─ HelloJava
   └─ Hello.java
```
	  
可以以如下两种方式编译`hello.java`

1、只输入可执行程序

![javaccmd](/pics/javaccmd.PNG)

2、输入可执行程序完整路径

![javaccmd](/pics/fulljavaccmd.PNG)

## JAVA_HOME

`JAVA_HOME`变量指定JDK安装目录，在这个目录下可以找到java命令及相关lib

**举例: 按如下安装目录设置`JAVA_HOME`**

```html
D:\Program Files
└─ Java
   └─ jdk-11.0.9
```

![javahome](/pics/javahome.PNG)

## CLASSPATH

`CLASSPATH`变量用于指定`.class`文件和`.jar`所在目录，JVM从变量中指定的目录中寻找`class`文件

### JVM如何查找`.class`文件

假设`CLASSPATH=.;F:\JAVA\HelloJava;D:\shared，当JVM在加载`abc.xyz.Hello`这个类时，会依次查找：
* <当前目录>\abc\xyz\Hello.class
* F:\JAVA\HelloJava\abc\xyz\Hello.class
* D:\shared\abc\xyz\Hello.class
如果JVM在某个路径下找到了对应的class文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。

### 如何设置`classpath`

设置搜索路径与操作系统相关。
在Windows系统上，用`;`分隔，带空格的目录用""括起来，可能长这样：
```
C:\work\project1\bin;C:\shared;"D:\My Documents\project1\bin"
```

在Linux系统上，用`:`分隔，可能长这样：
```
/usr/shared:/usr/local/bin:/home/liaoxuefeng/bin
```

`classpath`的设定方法有两种：
在系统环境变量中设置`classpath`环境变量，不推荐；
在启动JVM时设置`classpath`变量，推荐。
我们强烈不推荐在系统环境变量中设置`classpath`，那样会污染整个系统环境。
在启动JVM时设置`classpath`才是推荐的做法。实际上就是给java命令传入`-classpath`或`-cp`参数：

```
java -classpath .;F:\JAVA\HelloJava;D:\shared abc.xyz.Hello
```

或者使用-cp的简写：

```
java -cp .;F:\JAVA\HelloJava;D:\shared abc.xyz.Hello
```

没有设置系统环境变量，也没有传入`-cp`参数，那么JVM默认的`classpath`为.，即当前目录：

```
java abc.xyz.Hello
```

上述命令告诉JVM只在当前目录搜索Hello.class。

### 通过脚本或bat临时设置CLASSPATH

**Windows通过bat设置**

```html
创建auto.bat文件，在其末尾加入(CLASSPATH可以不设置，使用默认的当前目录就可以): 
set JAVA_HOME=D:\Program Files\Java
set PATH=%JAVA_HOME%\jdk-11.0.9\bin;%PATH%
set CLASSPATH=.;%JAVA_HOME%\jdk-11.0.9\lib;%JAVA_HOME%\jdk-11.0.9\lib\tools.jar
```

**Linux通过/etc/profile设置**

```bash
echo "# set java environment" >> /etc/profile
echo "JAVA_HOME=/usr/java/jdk" >> /etc/profile
echo "CLASS_PATH=.\$JAVA_HOME/lib/dt.jar:\$JAVA_HOME/lib/tools.jar" >> /etc/profile
echo "PATH=\$PATH:\$JAVA_HOME/bin:" >> /etc/profile
echo "export JAVA_HOME CLASS_PATH PATH" >> /etc/profile
```

## 使用离线文档

**下面以jdk-11.0.9_doc-all.zip为例进行说明**

* [下载java离线文档](https://www.oracle.com/java/technologies/javase-jdk11-doc-downloads.html)

  ![javadoc](/pics/javadoc.PNG)
  
* 将文档解压，为了方便将解压文件夹重命名为**javadocs**

  ![unzipjavadoc](/pics/unzipjavadoc.PNG)
  
* 使用浏览器打开文件夹中的**index.html**，就可以离线使用文档了

  ![openjavadoc](/pics/openjavadoc.PNG)

## 参考问献

https://www.liaoxuefeng.com/wiki/1252599548343744/1260466914339296
