**查找前后几行**
```shell
grep -C 3 love filename  显示filename文件中，love行上下3行内容（含love行）
grep -A 3 love filename  显示filename文件中，love行下3行内容（含love行）
grep -B 3 love filename  显示filename文件中，love行上3行内容（含love行）
```
