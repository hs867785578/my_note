
**socket通信大法好**
socket = socket.socket(family, type[, protocal])

1. family代表地址家族，一般为AF_UNIX，AF_INET和AF_INET6。AF_UNIX用于同一台机器上的进程通信，AF_INET用于IPV4协议的TCP和UDP，AF_INET6用于IPV6协议；
2. type代表套接字类型，一般为SOCK_STREAM，SOCK_DGRAM和SOCK_RAW。SOCK_STREAM为流式套接字，用于TCP通信，SOCK_DGRAM为数据报式套接字，用于UDP通信，SOCK_RAW为原始套接字，可以用于处理ICMP、IGMP等网络报文，这是普通套接字无法处理的;
3. protocal代表协议编号，默认为0。

## 1.进程间通信

1. 对于进程间的通信，可以使用python socket包，原理和**同一局域网下两台电脑的python通信**类似，但是把ip地址设置为""或者localhost就可以了，类型选择socket.AF_INET
2.linux下还可以使用socket中的socket.AF_UNIX来实现进程间的通信

[可以看这篇文章](https://blog.csdn.net/wangtaoking1/article/details/44494217)

## 2.同一局域网下两台电脑的python通信
类型选择socket.AF_INET

[可以看这篇文章](https://blog.csdn.net/qq_41605934/article/details/121689235?utm_medium=distribute.pc_relevant.none-task-blog-2~default~baidujs_baidulandingword~default-1-121689235-blog-120009756.235^v35^pc_relevant_increate_t0_download_v2_base&spm=1001.2101.3001.4242.2&utm_relevant_index=2)


## 3.公网下两台电脑的python通信

实现局域网到广域网的通信，首先需要一台连网的本地计算机和一**个服务器（比如购买的阿里云服务器，必须是连接英特网的）**。只要本地的计算机能ping通服务器的IP就行了（服务器是无法ping通本地的计算机的，原因前面已经介绍）。

由于局域网和广域网只能建立单向连接的socket通信，也就是说只能将服务器作为服务端来等待客户端的连接。所以我们将服务器作为服务端，在服务器上新建一个server.py文件（和局域网之间通信的服务端server.py没什么区别，只是绑定的IP为服务器自己的IP`server.bind(('192.168.27.238', 6688))`改为`server.bind(('220.181.111.188', 6688))`220.181.111.188是百度的IP，改为你的服务器IP），然后在本机建一个client.py文件，内容和局域网之间通信的client.py一致，只是连接的IP改为服务器的IP（`client.connect(('192.168.27.238', 6688))`改为`client.connect(('220.181.111.188', 6688))`，这里就不冗余的贴代码了）

[可以看这篇文章](https://blog.csdn.net/youand_me/article/details/83109238)
