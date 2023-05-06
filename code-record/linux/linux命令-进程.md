# Linux 命令——进程

> 该篇主要介绍 Linux 系统中关于进程信息的操作

### ps 显示当前系统进程的状态信息

查询包括进程 ID、进程名、CPU 和内存占用情况等

常用选项包括：

- ps aux：显示所有进程的详细信息
- ps -ef：与 ps aux 类似，但输出格式略有不同
- ps -ejH：显示进程树，按照层级关系显示进程之间的父子关系

如果需要查询指定进程信息可以结合 grep 命令进行查询

```shell
ps aux | grep nginx
```

上面的命令就是指定查询 nginx 的相关进程

### top 显示实时进程监控

它可以显示当前系统的进程信息、系统负载情况、内存使用情况等信息

输入 top 命令后 Linux 窗口会进入监视进程信息的窗口，在窗口内可以输入一些快捷键：

- q：退出 top，（也可以用 ctrl+z 退出）
- r：修改进程优先级
- k：终止指定进程
- Space：刷新当前信息
- 1：按照 CPU 占用率从高到低排序进程
- m：按照内存使用量从高到低排序进程
- t：切换进程信息的显示模式

### pstree 以树形结构展示进程之间的关系

### pidof 指定查询进程 PID

用于查找指定进程名称对应的进程 ID (PID)

基本语法： `pidof [options] process_name`

options 是可选参数，process_name 是待查找的进程名称

optios 常用命令：

- -s：仅返回一个 PID，如果有多个匹配项，则返回第一个匹配项的 PID
- -o：排除某些进程 ID，例如 -o 123,456 表示排除 PID 为 123 和 456 的进程
- -x：精确匹配进程名称，而不是模糊匹配

```shell
# 查找名称为 nginx 的进程的 PID，会返回多个PID
pidof niginx

# 只返回一个 PID
pidof -s nginx

# 排除 PID 为 123 和 456 的进程
pidof -o 123,456 nginx

# 精确匹配进程名称
pidof -x /usr/sbin/nginx
```

### kill 终止进程

它可以向指定的进程发送一个信号，让进程在接收到信号后终止运行

基本语法： `kill [signal] PID...`

signal 是信号名称或编号，PID 是要终止的进程 ID

常用 signal:(缺省默认值为-15)

- -1：HUP，重新读取配置文件
- -2：INT，中断进程
- -9：KILL，强制终止进程
- -15：TERM，通知进程终止
- -18：CONT，继续运行进程
- -19：STOP，暂停进程
- -20：TSTP，停止进程（交互式停止）

```shell
# 终止 PID 为 123 的进程
kill 123
kill -15 123

# 终止 PID 为 123 456 789 的进程
kill 123 456 789

# 暂停PID 为 123 的进程
kill -19 123
```
