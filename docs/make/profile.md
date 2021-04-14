## 什么是makefile

> make是一种文件更新管理工具，通常用于管理各种语言（C/C++/Python等）程序的编译工作。make可以自动追踪程序中发生了变化的文件，
> 并对这些发生变化的文件重新编译。实际上只要服务器可以运行shell命令，make就可以完成**文件发生变化，依赖这些文件的目标文件自动进行更新**的任务。
> 本手册讲解的是由**Richard Stallman**编写的GNU make。

**以C程序文件解释如何生成可执行文件**

![makeprofile](/pics/profile.jpg)

上面给出一个简单的C程序文件依赖关系图。如果更新了头文件common.h，make就会发现one.c和two.c的依赖文件发生变化，进而重新编译one.c和two.c，生成新的object文件one.o two.o，最后链接生成的可执行文件a.out
common.h---->one.c two.c---->one.o two.o---->a.out

## 规则

makefile中的规则定义如下所示
```html
targets: prerequisites
  recipe
  ...
  ...
```
* **targets** 为要生成的目标文件或要执行的动作（匿名**target**如`clean`）,以`C`语言为例，**targets** 为可执行文件或`object`文件
* **prerequisites** 为依赖文件（如`.c`文件等），用于生成目标文件。
* **recipe** 为一系列`shell`命令，描述如何根据依赖文件**prerequisites**生成目标文件**targets**。  
    **注意** **recipe**之前要以**Tab**开头，也可通过`.RECIPEPREFIX`变量设置成其他的字符。
