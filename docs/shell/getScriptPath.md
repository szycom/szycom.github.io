> 获取当前脚本所在路径
```bash
# 获取脚本工作目录
basedir=$(readlink -f $BASH_SOURCE)
basedir=${basedir%/*}
# 或则
# basedir=$(dirname ${basedir})
```
