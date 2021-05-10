## 查找包含指定内容的文件并对指定内容进行替换

**查找包含指定内容的文件
```shell
grep pattern -rl ${work_dir}
```

## 替换所有文件中指定内容

```shell
sed -i 's,origin-string,replace-string,g' `grep match-string -rl ${work_dir}` 
```


[常用命令](https://zhuanlan.zhihu.com/p/65515740)  
![grep](/pics/grep.JPG)
