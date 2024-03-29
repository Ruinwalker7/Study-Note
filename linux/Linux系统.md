# Linux系统

### Linux各部分组成

内核（Kernel）

Shell：将用户命令解释为内核可以接受的低级语言
-------
### Linux系统的组成

**内核：**

**整个操作系统的核心，内核控制整个计算机的运行，提供相应的硬件驱动程序、网络接口程序，并管理所有应用程序的执行。**

内核是模块化的



**Shell：**

**Shell**负责将**用户的命令**解释为**内核能够接受的低级语言**，并将操作系统相应的信息以用户能够理解的方式显示出来。



### Linux系统的概念

**账户**

Linux是一个分时多用户多系统的操作系统，任何要使用系统资源的用户，都必须向管理员申请一个账号，各种软件也需要创建账号



**Linux**的运行级别

0  停机。不要把系统的默认级别设置为0，否则系统不能正常启动。

1  用户模式。用于root用户对系统进行维护，不允许其他用户使用主机。

2  多用户模式。在该模式下不能使用NFS。

3  完全多用户模式。主机作为服务器时通常在该模式下。

4  未分配使用。

5  图形登陆的多用户模式。用户在该模式下可以进行图形界面的登陆。

6  重新启动。不要把系统的默认级别设置为6，否则不能正常启动。



### Linux的启动

1. 核内引导

   1. BIOS启动
   2. 操作系统接管，读入/boot下的内核文件

2. 运行`init`程序，从**`/etc/inittab`**中读取文件

   Linux允许为不同的场合，分配不同的开机启动程序，这就叫做"运行级别"（`runlevel`）。也就是说，启动时根据"运行级别"，确定要运行哪些程序。

3. 系统初始化

   **调用执行**`/etc/rc.d/rc.sysinit`**脚本**，该脚本是每一个运行级别都需要首先运行的脚本

   **完成一些系统初始化的工作，**包括**激活交换分区，检查磁盘，加载硬件模块以及其它一些需要优先执行任务。**

4. 启动守护进程

   **`rc.sysinit`**执行后，返回**`init`**继续其它的动作，通常接下来会执行到**`/etc/rc.d/rc`**程序。

      　`l5:5:wait:/etc/rc.d/rc 5`

   以**5**为参数运行`/etc/rc.d/rc5.d`目录下的所有的rc启动脚本

   **S**开头的启动脚本：将以**start**参数来运行。

    **K**打头的链：已经处于运行态，则将首先以**stop**为参数停止这些已经启动了的守护进程，然后再重新运行。





### 常用系统命令

格式: `passwd [username]`

功能: 修改系统用户密码

参数: [username]





## 进程管理

### Linux系统进程调用

**进程创建：**

```c
#include <unistd.h>
pid_t fork(void);
```

- fork()调用一次但是返回两次，子进程返回0，父进程返回子进程ID

- 子进程获得父进程所有资源的副本，但是并不共享

**获取ID：**

```c
 #include <unistd.h>
 pid_t getpid(void); //该进程的ID
 pid_t getppid(void); //该进程父进程的ID
```

**进程退出：**

```c
#include <unistd.h>
void _exit(int status);
void exit(int *status);
```

exit()会处理缓冲区内容，正常结束程序

_exit()会立即结束程序，不管理IO内容



**等待进程结束：**

```c
#include <sys/types.h>
#include <sys/wait.h>
pid_t wait(int *status);
```

- 返回值为子进程pid号
- status为子进程结束状态



```c
pid_t waitpid(pid_t pid, int *status, int options);
```

- pid<-1时等待pid绝对值的进程结束，pid=-1时等待任何子进程结束，pid=0时等待进程组识别码与目前进程相同的任何子进程。 pid>0 等待任何子进程识别码为 pid 的子进程。
- 执行成功返回子进程PID，失败返回-1



**僵死进程**

- 在一个进程调用了exit之后，该进程并非马上就消失掉，而是留下一个称为僵尸进程（Zombie）的数据结构。
- 几乎放弃了所有内存，仅保留一些基本信息
- 当一个进程已退出，但其父进程还没有调用系统调用wait（稍后介绍）对其进行收集之前的这段时间里，它会一直保持僵尸状态



**进程组ID**

```c
# include <unistd.h>
pid_t getpgid( pid_t pid);
pid_t getpgrp(void);  //取得当前进程的进程组ID
int setpgid(pid_t pid,pid_t pgid); //设置pid的进程组ID为pgid
```

- 用来取得参数 pid 指定进程所属的组识别码。如果参数 pid 为 0，则会取得目前进程的组识别码。

- 执行成功则返回组识别码，如果有错误则返回-1，错误原因存于 errno 中。



### Linux系统通信调用

常见信号

```c
SIGINT //中断信号
SIGALRM //接收alarm信号
```



**信号处理：**

- 忽略信号
- 调用默认处理
- 捕获信号：自定义处理方式



**信号的产生：**

```c
int raise( int sig );
```

- 给自己一个信号

```c
unisigned int  alarm( unsigned int seconds ); 
```

- 功能:在seconds秒后向自己发送一个SIGALRM信号

```c
int kill( pid_t pid,  int sig ); 
```

- 功能:发送一个信号给进程或者进程组

- 参数：pid:进程或进程组id，sig:信号标识



**信号的捕获和处理：**

```c
#include<signal.h>
void (*signal(int sig,void (*func)(int)))(int)
```

- 返回一个函数指针



### 信号量

**信号量的初始化：**

```c
#include <sys/sem.h>
int semget(key_t key, int num_sems, int sem_flags);
```

- key是整数值，不同进程可以通过key获得信号量
- num_sems通常为1
- 调用函数试图打开一个信号量集，如果该信号量集存在，则返回该信号量集的标识，如果该信号量集不存在，则函数返回错



**信号量的操作**

- sem_id是由semget()返回的信号量集标识符。
- sem_ops是指向将要操作的数组的指针。
- num_sem_ops是数组中的操作的个数。
- 返回值：0，如果成功。-1，如果失败



### 管道通信

- 写入的内容每次都添加在管道的末尾，并且每次都是从管道的头部读出数据，就像队列
- UNIX中的管道是一种先进先出（FIFO）的特殊文件

**创建pipe：**

```C
#include<unistd.h>
int pipe(int fd[2]);
```

- 用pipe函数创建的管道没有名字，是为了一次使用而创建的。
- 管道的两个描述字fdes[0]和fdes[1]是同时打开的。使用fork()创建子进程后，文件描述符被继承。这样，父进程从fd[1]写入的数据，子进程就可以从fd[0]中读出。
- 如果父进程创建两个子进程，都继承管道的描述符，两个子进程也可通过无名管道交换数据。一般的，有共同祖先的进程就有机会从同一个祖先继承这个祖先创建的无名管道的描述符，从而互相间通过无名管道通信。
- **fork()以后父子进程关闭不再需要的文件描述符。**





## 文件管理

程序在开始读写一个文件的内容前，必须在程序和文件之间建立**连接或通信通道**

UNIX中使用以下描述：

- 文件描述符
- 流



文件描述符表示为int类型的对象。例如标准输入对应文件描述符0，标准输出对应文件描述符1。

而流则表示为指向结构FILE的指针FILE* ，因此流也称为“文件指针”

对设备进行操作或特殊方式进行I/O操作时，必须使用文件描述符方式



```c++

# include <sys/types.h>
# include <sys/stat.h>
# include <fcntl.h>
//返回文件描述符，此方法只能写
int creat(const char* filename,int flags[,mode_t mode]); 
int link(const char* name1,const char* name2);
int unlink(const char* name);

//打开文件
int open(const char* filename,int flags[,mode_t mode]);
flag：取值：O_RDONLY(为只读而打开文件) ,O_WRONLY(为只写而打开文件),O_RDWR(为可读可写而打开文件),O_APPEND(文件位置移至文件尾，所有write操作写数据至文件尾部）
//关闭文件                                  
int close (int filedes);

//移动文件指针位置
off_t lseek(int filedes，off_t offset,int origin);
long int tell(int fd)

//加锁
int lockf(int filedes，int func,off_t size);

```





## Linux进程描述与控制

Linux中进程有一个`task_struct`结构描述

该结构描述了进程的状态

   **Linux内核用task_struct->state表示进程状态**
