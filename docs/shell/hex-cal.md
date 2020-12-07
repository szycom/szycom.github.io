**一种16进制计算方法**

以查看/proc/2206/maps下的so大小为例进行说明，计算方法如下
```
cat /proc/2206/maps | awk '{print $1}' | awk -F '-' '{printf "echo \"obase=16\;\$\(\(0x%s-0x%s\)\)\"|bc\n", $2, $1}'
```

**命令拆分**

```
# 使用awk获取操作元素
cat /proc/2206/maps | awk '{print $1}' | awk -F '-' '{....}'
# 使用echo计算16进制数据
echo "obase=16\;$((0xab-0xac))"|bc
```
