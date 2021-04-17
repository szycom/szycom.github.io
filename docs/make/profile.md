## 什么是makefile

> 本手册讲解的是由**Richard Stallman**编写的GNU make。make是一种文件更新管理工具，通常用于管理各种语言（C/C++/Python等）程序的编译工作。make可以自动追踪程序中发生了变化的文件，
> 并对这些发生变化的文件重新编译。实际上只要服务器可以运行shell命令，make就可以完成**文件发生变化，依赖这些文件的目标文件自动进行更新**的任务。
> 使用make首先要编写makefile，makefile中描述了目标文件和依赖文件之间的关系，并定义了根据依赖文件更新或生成目标文件的规则。
> 下面给出一个简单的示例，通过示例我们可以初步了解make的工作原理。

**一个简单的C语言编译示例**

                                                     ![cexample1](/pics/cexample1.jpg)

*  示例中描述了如何根据hello.c生成目标文件hello，我们只需要编写如下makefile
```make

hello: hello.o
    cc hello.o -o hello                       # Runs third

hello.o: hello.c
    cc -c hello.c -o hello.o                  # Runs second

hello.c:
    echo "int main() { return 0; }" > hello.c # Runs first
    
```

* 然后执行make hello就可以编译生成目标文件hello

```make

make hello

```

执行`make hello`大体执行流程如下：

![cexample1](/pics/cexample.jpg)

* `make hello`中指定目标`hello`作为目标文件，make搜索makefile中查找`hello`的生成规则，发现`hello`依赖`hello.o`
* make递归搜索`hello.o`的生成规则，发现`hello.o`依赖`hello.c`
* make递归搜索`hello.c`的生成规则，发现`hello.c`没有依赖，直接执行规则下指定的`echo`命令生成`hello.c`，搜索结束返回
* `hello.o`只依赖`hello.c`，搜索结束，执行`cc -c`命令生成`hello.o`，返回
* `hello`只依赖`hello.o`，搜索结束，执行`cc`命令生成`hello`，处理结束

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
