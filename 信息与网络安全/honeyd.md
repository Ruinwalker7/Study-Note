# Honeyd

## 安装教程

Honeyd官网下载：https://www.honeyd.org/releases/



1. 安装必要环境

```shell
sudo apt-get install libevent-dev libdumbnet-dev libpcap-dev libpcre3-dev libedit-dev bison flex libtool automake g++ gcc make cmake git
```



2. 安装依赖库

- libevernt安装

下载地址：https://libevent.org/，下载 2.1.x 版本

```shell
./configure -prefix=/usr/lib/libevent
make 
sudo make install
```



可能会报错没有openssl：

```
configure: error: openssl is a must but can not be found. You should add the directory containing `openssl.pc' to the `PKG_CONFIG_PATH' environment variable, or set `CFLAGS' and `LDFLAGS' directly for openssl, or use `--disable-openssl' to disable support for openssl encryption
```

参考以下步骤，在ubuntu中重新安装OpenSSL：https://blog.csdn.net/wu10188/article/details/124970453

解决方法：自己去官网下载OpenSSL源码，然后编译安装。

步骤：

1. 卸载自带的 OpenSSL （非必需）

```
sudo apt-get remove openssl
```

2. 去OpenSSL官网下载源码安装包：https://www.openssl.org/source/old/，需要注意的是要下载 1.1.1x 版本

3. 解压源码包，用终端进入其目录，

4. 输入指令编译安装

```shell
sudo ./config  --prefix=/usr/local/openssl
sudo make
sudo make install
```



将目录加到系统对应的环境变量里：

对所有用户有效修改 "/etc/profile" 可能需要重启系统才会生效

对个人有效则修改 "~/.bashrc"

修改.bashrc：

```shell
vim ~/.bashrc
```

向文件里面添加下列内容：

```shell
export PATH=$PATH:/usr/local/openssl/bin
export C_INCLUDE_PATH=$C_INCLUDE_PATH:/usr/local/openssl/include  
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:/usr/local/openssl/include
export LIBRARY_PATH=$LIBRARY_PATH:/usr/local/openssl/lib  
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/openssl/lib
```



回到 libevent 的文件夹中，打开终端：

```shell
 export PKG_CONFIG_PATH=/usr/local/openssl/lib/pkgconfig
```

这个是为了将 openssl.pc 文件的位置添加到 `PKG_CONFIG_PATH` 环境变量中去，然后重新安装 libevent





- libnet 安装

```shell
./configure
make
sudo make install
```



- [libpcap](http://www.tcpdump.org/release/) 安装

下载 1.x.x 好像都行

```shell
./configure
make
sudo make install
```



- ###### [zlib](https://www.zlib.net/) 安装

下载 1.3 应该就行

```shell
./configure
make
sudo make install
```



- [arpd](http://www.citi.umich.edu/u/provos/honeyd/arpd-0.2.tar.gz) 安装

```shell
./configure
make
sudo make install
```

可能问题解决：https://blog.csdn.net/weixin_30770783/article/details/95773730



因为 honeyd 不支持arp协议，所以我们需要一个专门的程序模拟arp协议，上面 arpd 可以完成这个任务，但是arpd会和ubuntu自带的冲突，所以可以使用下面这个开源程序

```shell
git clone https://github.com/quinot/choparp.git
#编译得到chopar可执行文件
gcc -o choparp choparp.c -lpcap
```






- libdnet 安装

```shell
./configure
make
sudo make install
```



### 安装 Honeyd

```shell
# 如果出现以下两个错误 需要进行库链接
# Error： Couldn't figure out how to access libc
sudo ln -s /lib64/libc.so.6 /usr/lib/libc.so
# Error：error while loading shared libraries: libdnet.1: cannot open shared object file: No such file or directory
sudo ln -s /usr/local/lib/libdnet.1 /usr/lib/libdnet.1


./configure
make
sudo make install
```



### 使用 Honeyd

启动honeyd程序

```
sudo honeyd -d -f /etc/honeypot/honeyd.conf
```

在之前编译choparp的目录下启动arp代理程序。

```
sudo ./choparp eth0 00:0c:29:08:56:f6 192.168.32.100
```

上述指令表示在eth0接口上监听目的IP为192.168.32.100的ARP请求，并为回复ARP响应，MAC地址为00:0c:29:08:56:f6。


实际在测试时，honeyd和choparp都运行在虚拟机里面，eth0是虚拟机的NAT接口，地址为192.168.32.131。我们想在eth0接口上再模拟一个IP地址为192.168.32.100的Cisco路由器。

