```
# 获取脚本工作目录
basedir=$(readlink -f $BASH_SOURCE)
basedir=${basedir%/*}
# 或则
# basedir=$(dirname ${basedir})
```
