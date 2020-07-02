## 为应用程序bma_hello_app编写启动脚本

以上示例三中我们编写的bma_hello_app并不会在系统启动时自行运行, 也不会在关机或重启时自行结束. 为此, 我们通常会为需要自动启动的程序编写启动(也含停止)脚本. 为此, 我们需要将上述编译并确认验证无误的bm_hello_app放置到bm_rootfs/usr/bin(或其它$PATH所可以指向的路径)中, 并确保bm_hello_app具有可执行属性(必要时可以通过chmod来添加). 另外, 在您确定的bm_hello_app的软件功能之后, 可以考虑执行`$STRIP bm_hello_app`来去除应用程序的符号表等调试信息, 再将其放入到bm_rootfs中.

目前可以参照的启动停止脚本基本都位于bm_rootfs/etc/init.d/中, 大体结构是需要判断$2是start, stop还是restart. 其中start/stop主要都是由start-stop-daemon(来自busybox)来引导的, 其帮助内容如下.

```
/var/volatile/tmp # start-stop-daemon
BusyBox v1.23.2 (2019-01-10 16:37:01 CST) multi-call binary.

Usage: start-stop-daemon [OPTIONS] [-S|-K] ... [-- ARGS...]

Search for matching processes, and then
-K: stop all matching processes.
-S: start a process unless a matching process is found.

Process matching:
        -u,--user USERNAME|UID  Match only this user's processes
        -n,--name NAME          Match processes with NAME
                                in comm field in /proc/PID/stat
        -x,--exec EXECUTABLE    Match processes with this command
                                in /proc/PID/{exe,cmdline}
        -p,--pidfile FILE       Match a process with PID from the file
        All specified conditions must match
-S only:
        -x,--exec EXECUTABLE    Program to run
        -a,--startas NAME       Zeroth argument
        -b,--background         Background
        -N,--nicelevel N        Change nice level
        -c,--chuid USER[:[GRP]] Change to user/group
        -m,--make-pidfile       Write PID to the pidfile specified by -p
-K only:
        -s,--signal SIG         Signal to send
        -t,--test               Match only, exit with 0 if a process is found
Other:
        -o,--oknodo             Exit with status 0 if nothing is done
        -v,--verbose            Verbose
        -q,--quiet              Quiet
```

* '-S'用来启动程序, '-K'用来结束程序, '-n'用来指定进程名, '-- ARGS'的含义是指'--'之后的参数为真正传递给实际进程的参数, 而'--'之前的为传递给start-stop-daemon的参数;
* 仅'-S'支持的参数有, '-a' 指定绝对路径名称, '-b' 指定进程以后台方式运行, '-N' 以指定的优先级运行, 当cpu高负载时进程可轮循到更多时间片, '-c' 切换到指定的用户运行, '-m'与'-p'一起指定保存pid到指定的路径中;
* 仅'-K'支持的参数有, '-s' 指定一个非SIGTERM的信号发送到进程, '-t' 仅供测试使用而不真地结束进程.

bma_hello_app的启动停止脚本如下, 记得将其放置在bm_rootfs/etc/init.d中, 我们在示例四中进行实际启动停止顺序的调整.

```
#!/bin/sh

BMA_HELLO_INTERVAL=10

set -e

case "$1" in
  start)
        echo -n "Starting bma_hello_app "
        start-stop-daemon -S -b -n bma_hello_app -a /usr/bin/bma_hello_app -- -i $BMA_HELLO_INTERVAL
        echo "done"
        ;;
  stop)
        echo -n "Stopping bma_hello_app "
        start-stop-daemon -K -n bma_hello_app
        echo "done"
        ;;
  restart)
        $0 stop
        $0 start
        ;;
  *)
        echo "Usage: syslog { start | stop | restart }" >&2
        exit 1
        ;;
esac

exit 0
```

本章[示例三 为应用程序bma_hello_app编写启动脚本]的实际操作示例: [编写脚本并安装](https://asciinema.org/a/238778) [模块上验证](https://asciinema.org/a/238789)

[![asciicast](https://asciinema.org/a/238778.svg)](https://asciinema.org/a/238778)

[![asciicast](https://asciinema.org/a/238789.svg)](https://asciinema.org/a/238789)
