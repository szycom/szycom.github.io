**查找前后几行**
```shell
grep -C 3 love filename  显示filename文件中，love行上下3行内容（含love行）
grep -A 3 love filename  显示filename文件中，love行下3行内容（含love行）
grep -B 3 love filename  显示filename文件中，love行上3行内容（含love行）
```

语法：
```hml
grep [option] pattern file

-A<显示行数>   --after-context=<显示行数>          #除了显示符合范本样式的那一列之外，并显示该行之后的内容。
-B<显示行数>   --before-context=<显示行数>         #除了显示符合样式的那一行之外，并显示该行之前的内容。
-C<显示行数>   --context=<显示行数>或-<显示行数>   #除了显示符合样式的那一行之外，并显示该行之前后的内容。
-l   --file-with-matches                          #列出文件内容符合指定的样式的文件名称。
-r   --recursive                                  #此参数的效果和指定“-d recurse”参数相同。
```
