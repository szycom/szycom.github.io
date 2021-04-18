## makefile介绍

  要想使用**make**管理我们程序编译工作，首先我们需要创建一个名为*makefile*的文件。*makefile*中定义了一系列规则**Rule**用来指导**make**编译和连接程序。  
  这一章节，我们将会讨论一个简单的*makefile*文件，文件中描述了如何指导**make**管理8个`.c`文件和3个`.h`文件编译和链接工作，最终产生如下效果：  
  * **make**重新编译*editor*
  * 发生变化的`.c`文件重新编译；
  * `.h`文件发生变化时，包含这些头文件的`.c`文件也将进行重新编译；
  * `.c`文件重新编译产生新的`.o`文件；  
  * `.o`文件发生变化，最终将会重新链接生成新的可执行文件*editor*。
  
  并申城开始规则中描述了两个方面的内容：  
* 目标文件(target)和依赖文件(prerequisites)之间的关系；
* 依赖文件发生变化后需要执行的命令序列（recipe）。

## 初识Rule

一个简单的makefile文件中包含的Rule，通常具有如下形式：
```html
target ... : prerequisites ...
  recipe
  ...
  ...
```

* *target*: 目标文件名，通常是程序编译过程中产生的文件，如可执行文件或`object`文件等，*target*也可以是一个动作，如下面例子中将要讨论的`clean`等；
* *prerequisites*：依赖文件，如`.c`文件等；
* *recipe*：一系列`shell`命令，**make**将*prerequisites*作为输入，利用这些命令，生成或更新目标文件*target*。  

**注意**：*recipe*之前要以**Tab**开头，也可通过`.RECIPEPREFIX`变量设置成其他的字符。

  上述Rule中`target ... : prerequisites`描述了`target`依赖哪些文件，`recipe`中描述执行哪些命令生成或更新`target`。除了Rule以为，makefile中还包含一些其他内容：变量、指令、注释等，这些将会在后续章节中逐一介绍。
