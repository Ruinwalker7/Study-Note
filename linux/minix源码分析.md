# MINIX3.30 源码分析

源码下载：https://github.com/Stichting-MINIX-Research-Foundation/minix

## 进程调度

**MINIX3 是一个基于分时的抢占式操作系统**



在`kernel/main.c`中，`kmain()`函数里面会调用进程调度初始化函数`proc_init();`，在这个函数之中，

会清空进程表，告诉所有槽进程表被清空并建立进程地址和指针的对应关系，对所有的系统进程也会这样做；之后会为每个cpu创建一个idle线程，当没有其他进程需要运行的时候，便运行idle线程。



在`kmain()`函数的末尾代码系统已经成功启动完毕，之后再也不会在回到这个函数中，调用`bsp_finish_booting()`函数开始准备进行进程调度，在CPU上启动定时器中断和时钟中断任务。



`bsp_finish_booting()`的结尾调用`switch_to_user()`函数，`switch_to_user()`函数在每次进程在每个时钟时都会调用，这个函数会先检测CPU中正在运行的进程的时间片是否用完，以及是否被挂起，如果是因为有高优先级的进程进入而导致进程挂起时会有以下两个行为：

1. 进程的CPU时间没有用完，调用`enqueue_head (register struct proc rp)`
2. 进程的CPU时间用完，则调用`enqueue (register struct proc rp)`

`enqueue()`函数中将进程添加到当前优先级的就绪队列尾部。该函数负责将进程插入到调度队列中。实际的调度策略为在sched()和pick_proc()中定义。

`enqueue_head()`函数将当前进程放入当前优先级的队列首部



之后再调用`pick_proc()`函数重新选择要运行的进程，这个函数会从优先级最高的队列开始遍历，返回找到的第一个进程，如果没有则返回null，之后会调用`idle()`函数，记录CPU空闲时间



进程调度初始化位于`/server/sched/main.c`中：

这个main程序会一直处于闲置知道被PM通知

程序首先调用函数开启消息服务

```c
/* SEF local startup. */
	sef_local_startup();
```

具体调度在（/servers/sched/schedule.c）

1、do_noquantum：

​	时间片用完后，如果本进程的优先级高于普通进程的最低优先级，则将进程降级，由内核将该进程挂到当前优先级队列的末尾

2、do_stop_scheduling：

​	接收来自PM和RS的所有消息，设置标志位表示schedule的插槽停止使用

3、do_start_scheduling:

​	接收来自PM和RS的所有消息，端口有效时根据消息设置该进程的schedproc的端点等内容，检查进程的类型，如果是初始化的，则设置优先级为可允许的最高优先级，分配完整时间片，如果是继承的，则设置优先级和时间片长度都与父进程的一致。设置标志位表明schedproc的插槽正在使用。接管调度的过程，利用内核调用回复消息填充进程当前的优先级和时间片。利用系统调用分配时间片。标记自己为新的调度。

4、do_nice:

​	保留请求进程的旧信息（包括旧的优先级、旧的最高优先级），将新的优先级设为最高优先级，由内核将其挂回队伍中，如果挂回过程中发生错误，则恢复旧的值

定时升级（balance_queues）

设置定时，每经过固定时间将所有没有时间片的进程的优先级提高一级