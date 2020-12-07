**vimTAB键的宽度设置**

在/etc/vimrc中添加如下代码
```
" ts是tabstop的缩写，设TAB宽4个空格
set ts=4
" 将TAB键转换成空格，这个设置在特定场合下要注意，如makefile中receip要以TAB开头
set expandtab
```
上述代码中"**\"+空格**"表示vimrc中的注释符
