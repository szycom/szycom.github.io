```
#!/bin/bash
# Program:
#   shows variable assignment and substitution
# History:
#   2020.08.05 zysun first release

# 变量定义时，在 = 前后不允许有空格
a=375
hello=$a

# =左边有空格, shell尝试运行“a”命令，参数为“=375”
# 运行出错提示：a: 未找到命令
a口=375
# =右边有空格，shell尝试运行“375”命令，把a设值为空的环境变量
# 运行出错提示：375: 未找到命令
a=口375

hello="A B C    D"
echo ${hello} # A B C D
# $hello是${hello}的简化写法
# 下面两种写法有不同输出，带双引号“$hello”会保留原有空白字符
echo $hello    # A B C D   , 只有一个空白字符
echo "$hello"  # A B C    D, 保留了原有空白字符
```
**export**
export目的是为了将本地变量传递给子进程
export的变量传递方向是：父进程----->子进程

**命令行参数**
$0就是脚本文件的名字
$1 是第一个参数
$2 为第2 个...

```
获取最后一个参数
args=$#            # Number of args passed.
lastarg=${!args}
# Note: This is an *indirect reference* to $args ...

# Or:       lastarg=${!#}          
# This is an *indirect reference* to the $# variable.
# Note that lastarg=${!$#} doesn't work.
```

**参考文献：**

http://tldp.org/LDP/abs/html/othertypesv.html
