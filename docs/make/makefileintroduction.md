## makefile初识

使用make首先要编写makefile，makefile由一系列规则组成，规则中描述了两个方面的内容：
1）目标文件(target)和依赖文件(prerequisites)之间的关系；
2）依赖文件发生变化后需要执行的命令序列（recipe）。

## makefile规则语法

```html
target ... : prerequisites ...
  recipe
  ...
  ...
```

* *target*: 目标文件，如可执行文件或`object`文件等，*target*也可以是一个动作（如`clean`）等；
* *prerequisites*：依赖文件，如`.c`文件等，用于生成目标文件；
* *recipe*：一系列`shell`命令，依赖文件*prerequisites*发生变化时自动触发执行，用于生成目标文件*targets*。  

**注意** *recipe*之前要以**Tab**开头，也可通过`.RECIPEPREFIX`变量设置成其他的字符。
