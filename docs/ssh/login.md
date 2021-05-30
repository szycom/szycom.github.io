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

### 生成密钥操作

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

### 将公钥传送到远程server上面

```bash
$ ssh-copy-id user@server
```

[How ssh-copy-id works]  
> ssh-copy-id uses the SSH protocol to connect to the target host and upload the SSH user key. The command edits the authorized_keys file on the server. It creates the .ssh directory if it doesn't exist. It creates the authorized keys file if it doesn't exist. Effectively, ssh key copied to server.  
> **It also checks if the key already exists on the server. Unless the -f option is given, each key is only added to the authorized keys file once.**

### 生成指定用户的ssh秘钥

参考[expect命令]
```shell
ssh $src_username@$src_host ssh-keygen -t rsa 
```

### 脚本将公钥传送到远程服务器

* 首先需要安装expect工具
* 利用expect实现免密登录

```shell
# 产生当前用户的ssh秘钥
sshKeygen(){
	local sshFile=$1

# 判断是否已经存在秘钥
	sudo test -f ${sshFile}
	if [ $? -eq 0 ];then
		return 0
	fi
# 使用expect生成秘钥
    /usr/bin/expect <<EOF
set timeout 60
spawn -noecho sudo ssh-keygen -f ${sshFile}
expect {
"Enter passphrase (empty for no passphrase):" { send "\r"; exp_continue} 
"Enter same passphrase again:" { send "\r"}
}
expect eof
EOF
}

# 测试是否已经将公钥拷贝到远端服务器，注意EOF前后不要有空格
sshCopyTest(){
    /usr/bin/expect <<EOF
set timeout -1
spawn -noecho sudo ssh $1 echo "test"
expect {
"*yes/no" { send "no\r"}
}
expect eof
EOF
}

# 将秘钥拷贝到远端服务器上
sshCopy(){
	local retVal=0
	local sshFile=$1

	retVal=`sshCopyTest $2 | grep "test" | grep -v spawn|wc -l`
	if [ ${retVal} -eq 1 ];then
		return 0
	fi

    /usr/bin/expect <<EOF
set timeout -1
spawn -noecho sudo ssh-copy-id -f -i ${sshFile} $2
expect {
"]"  { send "\r"}
"*yes/no" { send "yes\r"; exp_continue }
"*password:" { send "$3\r" }
}
expect eof
EOF
}
```

### 执行ssh user@server commands命令ssh将哪个公钥发送给远端服务器

* 执行ssh user@server时，使用当前用户的公钥 /home/user/.ssh/id_rsa.pub
* 执行sudo ssh user@server时，使用root的公钥 /root/.ssh/id_rsa.pub

**所以当使用ssh公钥登录服务器失败时，请检查执行ssh的用户是否正确

可用打开ssh客户端调试信息查看 [SSHD调试]
```shell
# 不使用sudo时
[admin@localhost ~]$ ssh -v admin@110.1.21.201 echo "test"
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 58: Applying options for *
debug1: Connecting to 110.1.21.201 [110.1.21.201] port 22.
debug1: Connection established.
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_rsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_rsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_dsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_dsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_ecdsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_ecdsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_ed25519 type -1
debug1: key_load_public: No such file or directory
debug1: identity file /home/admin/.ssh/id_ed25519-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_7.4
debug1: Remote protocol version 2.0, remote software version OpenSSH_7.4
debug1: match: OpenSSH_7.4 pat OpenSSH* compat 0x04000000
debug1: Authenticating to 110.1.21.201:22 as 'admin'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ecdsa-sha2-nistp256
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: curve25519-sha256 need=64 dh_need=64
debug1: kex: curve25519-sha256 need=64 dh_need=64
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: Server host key: ecdsa-sha2-nistp256 SHA256:dAuH8C+jJIlI6PCCq781NtIG6cyrql7BIXkFl/aU1vA

# 使用sudo时
[admin@localhost ~]$ sudo ssh -v admin@110.1.21.201 echo "test"
OpenSSH_7.4p1, OpenSSL 1.0.2k-fips  26 Jan 2017
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 58: Applying options for *
debug1: Connecting to 110.1.21.201 [110.1.21.201] port 22.
debug1: Connection established.
debug1: permanently_set_uid: 0/0
debug1: identity file /root/.ssh/id_rsa type 1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_rsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_dsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_dsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ecdsa type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ecdsa-cert type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ed25519 type -1
debug1: key_load_public: No such file or directory
debug1: identity file /root/.ssh/id_ed25519-cert type -1
debug1: Enabling compatibility mode for protocol 2.0
debug1: Local version string SSH-2.0-OpenSSH_7.4
debug1: Remote protocol version 2.0, remote software version OpenSSH_7.4
debug1: match: OpenSSH_7.4 pat OpenSSH* compat 0x04000000
debug1: Authenticating to 110.1.21.201:22 as 'admin'
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received

```

# 参考资料

[SSH原理与运用] [图解SSH原理] [SSH Brief Introduction] [ssh 公钥验证无效1] [SSHD调试] [expect命令] [How ssh-copy-id works]

[SSH原理与运用]:http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html
[图解SSH原理]:https://www.jianshu.com/p/33461b619d53
[SSH Brief Introduction]:https://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch02_04.htm#ch02-92834.html
[ssh 公钥验证无效1]:https://www.jianshu.com/p/f454f79b6052
[SSHD调试]:https://blog.csdn.net/ttyy1112/article/details/115399705
[expect命令]:https://www.cnblogs.com/lixigang/articles/4849527.html
[How ssh-copy-id works]:https://www.ssh.com/academy/ssh/copy-id
