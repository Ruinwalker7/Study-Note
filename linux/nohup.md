# linux nohup命令

在深度学习中，经常会遇到需要远程训练的情况，如果使用普通的终端命令进行训练，一旦终端断开，训练的进程也会被终止，所以我们需要使用`nohup`命令将进程与终端不绑定在一起

**nohup** 英文全称 no hang up（不挂起）

```shell
 nohup Command [ Arg … ] [　& ]
```

**参数说明**：

**Command**：要执行的命令。

**Arg**：一些参数，可以指定输出文件。

**&**：让命令在后台执行，终端退出后命令仍旧执行。



`nohup` 指令只是让运行命令与终端无关，&才是实现后台运行的指令



### 常用示例

```
nohup /root/runoob.sh > runoob.log 2>&1 &
```

**2>&1** 解释：

将标准错误 2 重定向到标准输出 &1 ，标准输出 &1 再被重定向输入到 runoob.log 文件中。

- 0 – stdin (standard input，标准输入)
- 1 – stdout (standard output，标准输出)
- 2 – stderr (standard error，标准错误输出)





### 如何关闭后台的程序

需要使用查找进程的指令先找到进程的PID

```cmd
ps -aux | grep 进程关键字
ex:
ps -aux | grep "runoob.sh" 

# kill进程
kill -9  进程号PID
```



### 和python配合使用

python输出的时候会有一个缓冲，因此无法实时将输出存储到log文件中，因此需要在python运行命令中添加`-u`参数，例如：

```shell
nohup python -u train.py > train.py 2>&1 &
```

