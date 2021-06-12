## tar命令打包指定目录下的所有文件
参考[1]

```shell
# 打包指定目录下的文件，不包含目录本身，不包含绝对路径
test@master:~$ tar -cvf test.tar -C /home/test/test_dir .
./
./b.txt
./a.txt

# 打包指定目录下的文件，包含目录本身，不包含绝对路径[1]
test@master:~$ tar -cvf test.tar -C /home/test test_dir
test_dir/
test_dir/b.txt
test_dir/a.txt

# 打包指定目录下的文件，包含目录本身，不包含绝对路径
test@master:~$ tar -cvf test.tar -C /home/test test_dir/*
test_dir/a.txt
test_dir/b.txt

# 打包指定目录下的文件，包含目录本身，包含绝对路径
test@master:~$ tar -cvf test4.tar /home/test/test_dir/*
/home/test/test_dir/a.txt
/home/test/test_dir/b.txt

# 对比打包内容如下

# 只包含文件本身
test@master:~$ tar -tf test.tar
./
./b.txt
./a.txt

# 包含文件和目录
test@master:~$ tar -tf test2.tar
test_dir/
test_dir/b.txt
test_dir/a.txt

# 包含文件和目录
test@master:~$ tar -tf test3.tar
test_dir/a.txt
test_dir/b.txt

# 包含文件和目录（绝对路径）
test@master:~$ tar -tf test4.tar
home/test/test_dir/a.txt
home/test/test_dir/b.txt

# 对比解压后的文件内容如下

# 只包含文件
test@master:~$ tar -xf test.tar -C test
test@master:~$ ls -al test
total 8
drwxrwxr-x 2 test test 4096 6月  12 15:12 .
drwxr-xr-x 8 test test 4096 6月  12 15:46 ..
-rw-rw-r-- 1 test test    0 6月  12 15:12 a.txt
-rw-rw-r-- 1 test test    0 6月  12 15:12 b.txt

# 包含目录
test@master:~$ tar -xf test2.tar -C test2
test@master:~$ ls -al test2
total 12
drwxrwxr-x 3 test test 4096 6月  12 15:47 .
drwxr-xr-x 8 test test 4096 6月  12 15:46 ..
drwxrwxr-x 2 test test 4096 6月  12 15:12 test_dir

# 包含目录
test@master:~$ tar -xf test3.tar -C test3
test@master:~$ ls -al test3
total 12
drwxrwxr-x 3 test test 4096 6月  12 15:47 .
drwxr-xr-x 8 test test 4096 6月  12 15:46 ..
drwxrwxr-x 2 test test 4096 6月  12 15:47 test_dir

# 包含目录（绝对路径）
test@master:~$ tar -xf test4.tar -C test4
test@master:~$ ls -al test4/
total 12
drwxrwxr-x 3 test test 4096 6月  12 15:59 .
drwxr-xr-x 9 test test 4096 6月  12 15:59 ..
drwxrwxr-x 3 test test 4096 6月  12 15:59 home
 
```

[1]:https://www.hangge.com/blog/cache/detail_2599.html
