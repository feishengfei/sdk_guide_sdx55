## 示例二 应用程序bma_hello_app

在本章示例中, 将逐步演示编写一个可长期运行在后台的应用, 尽管它的实际功能只是周期性地将信息输出到syslog中.

> step 1 在SDK的根目录下创立单独的文件夹bma_hello_app, 并在其中建立bma_hello_app.c

``` c
#include <stdio.h>
#include <unistd.h>     /*getopt*/
#include <stdlib.h>     /*atoi*/
#include <syslog.h>
#include <signal.h>

static int g_interval = 10;
static int g_stop = 0;

static void sig_handler(int sig)
{
    switch(sig) {
        case SIGTERM:
        case SIGINT:
            fprintf(stderr, "signal(%d) captured, leave\n", sig);
            g_stop = 1;
            break;
        default:
            break;
    }
}

int main(int argc, char *argv[])
{
        int i = 0, j = 0;
        int opt = 0;
        struct sigaction sa;

        while ((opt = getopt(argc, argv, "i:")) != -1) {
                switch (opt) {
                        case 'i':
                                g_interval = atoi(optarg);
                                break;
                        default: /* '?' */
                                fprintf(stderr, "Usage: %s [-i secs]\n", argv[0]);
                                exit(EXIT_FAILURE);
                }
        }

        sa.sa_flags = 0;
        sa.sa_handler = sig_handler;
        sigaction(SIGTERM, &sa, NULL);
        sigaction(SIGINT, &sa, NULL);

        openlog(argv[0], LOG_PID, LOG_DAEMON);

        while(!g_stop) {
                if(0 == (++i)%g_interval) {
                        syslog(LOG_INFO, "%s hello %8d", __FILE__, ++j);
                }
                sleep(1);
        }

        closelog();
        return 0;
}
```

> step 2 在bma_hello_app中编写如下Makefile, 注意下面命令行的行首为tab而非空格.

``` makefile
TARGET=bma_hello_app

all:$(TARGET)

SRCS += bma_hello_app.c

OBJS = $(SRCS:%.c=%.o)

$(TARGET): $(OBJS)
        $(CC) $^ $(LDFLAGS) \
                -o $@

clean:
        rm -fr $(TARGET) *.o
```

> 编译与推送测试

执行`make`进行编译, 随后将编译服务器中的应用程序同步到本地, 再通过`adb push` 推送到板子上进行验证. 注意随时关注应用程序的校验和以确保应用程序前后保持一致.

本章[示例二 应用程序bma_hello_app]的[操作示例](https://asciinema.org/a/238773)
[![asciicast](https://asciinema.org/a/238773.svg)](https://asciinema.org/a/238773)
