> ssh是一种安全网络传输协议，ssh客户端会自动加密要传输的数据，收到消息后ssh服务端自动对数据进行解密。

## ssh远程登录

```bash
  # 以用户名user，登录远程server
  $ ssh user@server
  # 如果本地用户名与远程用户名一致，登录时可以省略用户名。
  $ ssh server
  # SSH的默认端口是22，也就是说，你的登录请求会送进远程主机的22端口。使用p参数，可以修改这个端口。
  # 这条命令表示，ssh直接连接远程server的2222端口。
  $ ssh -p 2222 user@server
```

## ssh免密登陆方式

> 可以通过密码登陆到远程server，也可以通过公钥登陆到远程server。
> 使用密码登录，每次都必须输入密码，非常麻烦。SSH还提供了公钥登录，可以省去输入密码的步骤。
> 所谓"公钥登录"，就是用户将自己的公钥储存在远程server上。

**生成密钥操作**

```bash
$ ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
$ chmod 0600 ~/.ssh/authorized_keys

ssh-keygen是用于生产密钥的工具。
-t：指定生成密钥类型（rsa、dsa、ecdsa等）
-P：指定passphrase，用于确保私钥的安全
-f：指定存放密钥的文件（公钥文件默认和私钥同目录下，不同的是，存放公钥的文件名需要加上后缀.pub）
首先看下面~/.ssh中的四个文件：

SSH-涉及文件
id_rsa：保存私钥
id_rsa.pub：保存公钥
authorized_keys：保存已授权的客户端公钥
known_hosts：保存已认证的远程server公钥

当远程server的公钥被接受以后，它就会被保存在文件$HOME/.ssh/known_hosts之中。
下次再连接这台主机，系统就会通过known_hosts对远端server进行认证。
每个SSH用户都有自己的known_hosts文件，此外系统也有一个这样的文件，
通常是/etc/ssh/ssh_known_hosts，保存一些对所有用户都可信赖的远程server的公钥。
```

**将公钥传送到远程server上面：**

```bash
$ ssh-copy-id user@server
```

# 参考文献
http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html
https://www.jianshu.com/p/33461b619d53
https://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch02_04.htm#ch02-92834.html
