## 什么是makefile

> makefile总体来说就是一系列规则的总和，规则中定义了目标文件、依赖文件以及根据依赖文件生成目标文件的命令。
> make可以自动发现发生变化的文件，当执行make命令时根据makefile定义的规则自动对有影响的目标文件进行重新编译。
> make不仅仅可以编译各种语言类型的应用程序，实际上只要编译服务器可以运行shell命令，make就可以完成类似更新一个文件后，依赖这个文件的其他文件可以自动进行更新的任务。

**以C程序文件解释如何生成可执行文件**

更新了头文件a.h，如果a.c b.c中包含了头文件a.h，则a.c和b.c也会自动更新，相应的object文件a.o b.o也会更新，最后链接生成的可执行文件out也会进行
a.h---->a.c b.c---->a.o b.o---->out

## 规则

makefile中的规则定义如下所示
```html
target ... : *prerequisites ...
  recipe
  ...
  ...
```
* **target**为要生成的文件或一个动作（匿名target如‘clean’）,已C为例，target为可执行文件名或object文件名
* **prerequisites**为源文件，作为输入生成目标文件，如.c文件等
* **recipe**为一系列shell命令，执行如何根据依赖文件prerequisites生成目标文件target。**注意**recipe之前要以**Tab**开头。也可通过`.RECIPEPREFIX`变量设置成其他的字符。

