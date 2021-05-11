## [sed命令格式及常用选项][1]
```html
sed的命令格式：sed [options] 'command' file(s);
常用options:
 a\ 在当前行下面插入文本;
 i\ 在当前行上面插入文本;
 c\ 把选定的行改为新的文本;
```

## 查找包含指定内容的文件并对指定内容进行替换

**查找包含指定内容的文件
```html
grep pattern -rl ${work_dir}
```

## 替换所有文件中指定内容

```html
sed -i 's,origin-string,replace-string,g' `grep match-string -rl ${work_dir}` 
```

## 查找包含指定内容的文件并在查找到的行上面插入文本进行
```html
示例1
i\命令 将 this is a test line 追加到以test开头的行前面：
sed '/^test/i\this is a test line' file
 
示例二
sed -i '/pattern/i\insterString' `grep pattern -rl ${work_dir}`
```

[常用命令][2]  
![grep](/pics/grep.JPG)

[1]:https://www.linuxprobe.com/linux-sed-command.html
[2]:https://zhuanlan.zhihu.com/p/65515740
