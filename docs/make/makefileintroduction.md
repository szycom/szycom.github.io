## makefile介绍

  要想使用**make**管理程序编译工作，首先需要创建一个名为*makefile*的文件。*makefile*中定义了一系列规则**Rule**用来指导**make**编译和连接程序。  
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

## 一个简单的makefile实例

  这是一个简单的makefile文件，makefile文件中描述如何编译生成`C`语言可执行程序**edit**。**edit**由8个`.c`文件和3个`.h`文件编译而成。
  
```html
edit : main.o kbd.o command.o display.o \    # 如果一行文本太长，我们可以使用反斜杠`\`将比较长的行分成多行。
		insert.o search.o files.o utils.o
	cc -o edit main.o kbd.o command.o display.o \
		insert.o search.o files.o utils.o
main.o : main.c defs.h
	cc -c main.c

kbd.o : kbd.o defs.h command.h
	cc -c kbd.c

command.o : command.c defs.h command.h
	cc -c command.c 

display.o : display.c defs.h buffer.h 
	cc -c display.c 

insert.o : insert.c defs.h buffer.h 
	cc -c insert.c 

search.o : search.c defs.h buffer.h 
	cc -c search.c 
files.o : files.c defs.h buffer.h command.h 
	cc -c files.c 

utils : utils.c defs.h 
	cc -c utils.c 

clean : 
	rm edit main.o kbd.o command.o display.o insert.o search.o \
	files.o utils.o 
```

在这个*makefile*文件所在目录执行`make`命令就可以编译生成**edit**可执行文件。
```html
make 
```

执行`make clean`就可以删除可执行文件**edit**和所有的`.o`文件
```html
make clean
```
这个*makefile*文件中，可执行文件**edit**和Object文件**main.o**、**kbd.o**等称之为target。源文件**main.c**和头文件**def.h**等称之为prerequisites。
实际上`.o`文件既是target又是prerequisites，比如`main.o`既是可执行文件**edit**的prerequisites又是**main.c**的target。shell命令`cc -c main.c`等
称之为**Recipe**。如果target是一个文件，且它的任何一个prerequisites发生变化，make就会重新编译或重新链接生成新的target。这个示例中**edit**依赖8个Object文件，
其中**main.o**依赖源文件**main.c**和头文件**def.h**。如果头文件**def.h**发生了变化，**main.c**会被重新编译生成新的**main.o**，**main.o**发生变化导致
重新链接生成新的**edit**。

这个示例中**clean**是一个比较特殊的target：  
* 不是一个实际存在的文件；
* 没有依赖文件prerequisites；
* 不是其他target的依赖文件。  
	
**clean** 只是一个动作标识，表示要执行哪些命令。 **clean**没有依赖文件也不是其他target的依赖文件，所以make不会主动执行**clean**下的Recipe指定的命令。
我们需要像这样`make clean`明确告知make执行**clean**下的命令。像这种特殊的target称之为伪目标(Phony target)，后续章节会有介绍。
	
## 读取处理makefile

上面讨论了一个简单的**makefile**文件，在*makefile*文件所在目录下执行命令：
```html
make
```
**make**会在当前目录下搜索名为**makefile**的文件，读取文件内容。命令中我们没有明确指定要生成或更新的target，所以**make**按顺序逐个查找文件中定义的Rule，
并将第一个找到的target（不是以**.** 开头target，**.** 开头的是伪target，不在搜索范围中）作为要更新或生成的target。
这个target我们称之为**缺省目标**(default goal)。缺省目标也可以通过变量**.DEFAULT_GOAL** 进行设置。在这个示例中**edit**就是找到的缺省目标文件。
**make**开始处理**edit**目标文件的生成或更新工作，具体流程如下：
* **make**查找**edit**所有的依赖文件prerequisites，**edit**有8个`.o`依赖文件;
* **make**执行所有依赖的`.o`文件对应的Rule。`.o`文件对应的Rule描述了如何根据`.c`文件和`.h`文件重新编译`.o`文件;
* 如果`.o`文件依赖的`.c`文件或`.h`文件发生了变化，执行`.o`目标文件下Recipe指定的命令重新编译生成新的`.o`文件；
* 若果**edit**依赖的任一`.o`文件发生了变化，执行**edit**下Recipe中的命令重新链接生成新的**edit**文件。

