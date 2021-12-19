待执行计划

- [ ] 2020-12-17 
  整理git基本操作
- [ ] 2020-12-17
  整理shell基础知识
- [ ] 2021-01-02
  整理makefile基础知识
- [ ] 2021-01-02
  整理rpm基础知识
  
 [网卡收包](https://blog.csdn.net/weixin_44062361/article/details/108725060?utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.baidujs&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7EBlogCommendFromMachineLearnPai2%7Edefault-1.baidujs)

[网卡收包流程](https://cloud.tencent.com/developer/article/1030881?from=article.detail.1628161)

[网卡收包2](https://www.cnblogs.com/muahao/p/10861771.html)

[DPDK之网卡收包流程](https://zhuanlan.zhihu.com/p/65424382)

[25 张图，拆解 Linux 网络包发送过程](https://cloud.tencent.com/developer/article/1836743?from=article.detail.1836742)

[图解Linux网络包接收过程](https://cloud.tencent.com/developer/article/1757691?from=article.detail.1030881)

[LINUX网络子系统中DMA机制的实现](https://cloud.tencent.com/developer/article/1628161)

[LINUX内核网络性能优化](http://kerneltravel.net/blog/2021/ljr_network17/)

[网卡驱动收发包过程](https://blog.csdn.net/hz5034/article/details/79794615?utm_source=copy)

[数据包接收系列 — NAPI的原理和实现](https://blog.csdn.net/zhangskd/article/details/21627963)
[openconfig](https://blog.csdn.net/qq_27923047/category_10402228.html)
[grpc](https://grpc.io/docs/what-is-grpc/introduction/)

$ c:/users/szy/appdata/local/programs/python/python39/python.exe -m pip install grpcio
Collecting grpcio
  Downloading grpcio-1.41.1-cp39-cp39-win_amd64.whl (3.2 MB)
Requirement already satisfied: six>=1.5.2 in c:\users\szy\appdata\local\programs\python\python39\lib\site-packages (from grpcio) (1.15.0)
Installing collected packages: grpcio
Successfully installed grpcio-1.41.1

protobuf-3.19.1-py2.py3-none-any.whl
grpcio-1.41.1-cp36-cp36m-win_amd64.whl



from socketserver import BaseRequestHandler, TCPServer
from threading import Thread
import socket


class EchoHandler(BaseRequestHandler):
    def handle(self):
        print('Got connection from', self.client_address)
        while True:

            msg = self.request.recv(8192)
            if not msg:
                print('Close connection', self.client_address)
                break
            # self.request.send(msg)


if __name__ == '__main__':
    NWORKERS = 16
    serv = TCPServer(('0.0.0.0', 20000), EchoHandler, bind_and_activate=False)
    serv.socket.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, True)
    # Bind and activate
    serv.server_bind()
    serv.server_activate()
    for n in range(NWORKERS):
        t = Thread(target=serv.serve_forever)
        t.daemon = True
        t.start()
    serv.serve_forever()

import socket
# 1.创建socket
tcp_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# 2. 链接服务器
server_addr = ("192.168.26.11", 20000)
tcp_socket.connect(server_addr)

# 3. 发送数据
send_data = input("请输入要发送的数据：")
tcp_socket.send(send_data.encode("gbk"))

temp = int(0)
while True:
    tcp_socket.send(send_data.encode("gbk"))
    # temp = temp + 1
    # recv_data = tcp_socket.recv(1024)  # 接收1024个字节
    # if recv_data:
    #     print('接收到的数据为:', recv_data.decode('gbk'))
    #     send_data_test = send_data + 'client' + str(temp)
    #     tcp_socket.send(send_data_test.encode("gbk"))
    # else:
    #     break

# 4. 关闭套接字
tcp_socket.close()
