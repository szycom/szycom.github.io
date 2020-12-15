
## 设置设备启动后自动执行shell脚本

### 开机启动顺序

* 加载内核

启动 init(/etc/inittab)
内核启动的第一个用户级别的进程，其 pid 始终为 1，其它的开机启动脚本都是通过是通过这个进程来启动的。

* 执行 /etc/rc.d/rc.sysinit

这是 init 执行的第一个脚本，这个脚本主要工作是进行系统的初始化，如：设置系统字体、启动 swapping、设置主机名、装载声卡模块等。

* 执行 /etc/rc.d/rc*.d（rc0.d、rc1.d、rc2.d…rc6.d）
这一步会运行各个运行级别的脚本。这些运行脚本是指通过 chkconfig 命令配置的开机启动各个级别所要要执行的程序。

* 执行 /etc/rc.d/rc.local（就是 /etc/rc.local）
在各级别服务启动后，会执行该文件，如果不需要把所要执行的脚本配置为系统服务，也可以把所需执行的命令写到这个文件中，相比来说更为简单方便。

* /sbin/mingetty，等待用户登录

### 方法一 chkconfig设置启动脚本

已启动脚本为codis-install.sh为例进行说明
```bash
  # copy启动脚本到/etc/init.d/目录
  cp codis-install.sh /etc/init.d/
  # 设置执行权限
  cd /etc/init.d/
  chmod u+x codis-install.sh
  # 将可执行文件添加到/sbin目录下
  ln -s /etc/init.d/codis-install.sh /sbin/codis-install.sh
  # 设置设备启动后自动指定脚本
  systemctl endable codis-install.sh
```

**codis-install.sh的开头必须如下设置**

```vim
#!/bin/bash
#chkconfig: 2345 80 90
#description:codis-install.sh

...

```

> chkonfig后面是启动级别和优先级，description后面是服务描述。如上面脚本意思是，
> 服务必须在运行级2，3，4，5下被启动或关闭，启动的优先级是90，停止的优先级是10。
> 优先级范围是0－100，数字越大，优先级越低。
> 注意：不添加以上内容的话添加启动项时会提示service myservice does not support chkconfig

### 方法二 修改/etc/rc.d/rc.local文件

下面已添加一个脚本codis-install.sh为例进行说明

```bash
# 首先给rc.local添加可执行权限
chmod +x  /etc/rc.d/rc.local

# 确保codis-install.sh脚本具有可执行权限
chmod +x /etc/init.d/codis-install.sh
```
然后编辑/etc/rc.d/rc.local文件，添加一行代码

```vim
sh /etc/init.d/codis-install.sh
```

### 方法三 使用定时任务crontab

```shell
# 添加定时任务
crontab -e

# 在打开的文件中输入定时脚本
@reboot /etc/init.d/codis-install.sh
```

## 参考资料

[开机启动顺序](https://www.jianshu.com/p/e1442913eb0e)

[crontab设置开机启动脚本](https://www.jianshu.com/p/ff2f0a2b481c)
