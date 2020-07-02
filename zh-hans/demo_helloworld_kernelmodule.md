
## 内核模块bma_hello_module

在本章示例中,　将逐步演示编写一个可动态安装与卸载的hello module内核驱动.

> step 1 在bm_kernel/msm-3.18/samples/中新建loadable文件夹, 并在其中新建bma_hello_module.c, 其内容如下

```
#include <linux/kernel.h>
#include <linux/init.h>
#include <linux/module.h>

static int __init bma_hello_init(void)
{
        printk(KERN_ERR "%s bma module hello", __FUNCTION__);
        return 0;
}

static void __exit bma_hello_exit(void)
{
        printk(KERN_ERR "%s bma module bye", __FUNCTION__);
}

module_init(bma_hello_init);
module_exit(bma_hello_exit);
```

> step 2 在bm_kernel/msm-3.18/samples/loadable/中新建Makefile, 其内容如下

```
obj-$(CONFIG_SAMPLE_HELLO_MODULE) += bma_hello_module.o
```

> step 3 在bm_kernel/msm-3.18/samples/下的Makefile中增加step 1中新建的loadable文件夹

```
diff --git a/bm_kernel/msm-3.18/samples/Makefile b/bm_kernel/msm-3.18/samples/Makefile
index 1a60c62..43713b4 100644
--- a/bm_kernel/msm-3.18/samples/Makefile
+++ b/bm_kernel/msm-3.18/samples/Makefile
@@ -1,4 +1,4 @@
 # Makefile for Linux samples code

 obj-$(CONFIG_SAMPLES)  += kobject/ kprobes/ trace_events/ \
-                          hw_breakpoint/ kfifo/ kdb/ hidraw/ rpmsg/ seccomp/
+                          hw_breakpoint/ kfifo/ kdb/ hidraw/ rpmsg/ seccomp/ loadable/
```

> step 4 在bm_kernel/msm-3.18/samples/下的Kconfig中增加step 2中CONFIG_SAMPLE_HELLO_MODULE的配置项

```
diff --git a/bm_kernel/msm-3.18/samples/Kconfig b/bm_kernel/msm-3.18/samples/Kconfig
index 6181c2c..5d1eae8 100644
--- a/bm_kernel/msm-3.18/samples/Kconfig
+++ b/bm_kernel/msm-3.18/samples/Kconfig
@@ -63,4 +63,10 @@ config SAMPLE_RPMSG_CLIENT
          to communicate with an AMP-configured remote processor over
          the rpmsg bus.

+config SAMPLE_HELLO_MODULE
+       tristate "Build hello module sample -- loadable modules only"
+       depends on m
+       help
+         This builds kernel loadable module hello
+
 endif # SAMPLES
```

需要说明的是, 这里由于是可以选择为y/n/m, 因此项目为"tristate"而非"bool", "bool"只支持y(编译到builtin)或n(不参与编译); "depends on m"表明依赖于可加载驱动模块.

> step 5 手动进行menuconfig打开SAMPLE_HELLO_MODULE

这一步操作后需将bm_kernel/msm-3.18/build/.config中新打开的项目分别更新到bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig与bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig,　更个config模板更新之后的内容如下

```
diff --git a/bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig b/bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig
index 4d2627e..82f8a38 100644
--- a/bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig
+++ b/bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig
@@ -413,6 +413,8 @@ CONFIG_IPC_LOGGING=y
 CONFIG_BLK_DEV_IO_TRACE=y
 CONFIG_PANIC_ON_DATA_CORRUPTION=y
 CONFIG_DEBUG_USER=y
+CONFIG_SAMPLES=y
+CONFIG_SAMPLE_HELLO_MODULE=m
 CONFIG_CRYPTO_DEV_QCRYPTO=y
 CONFIG_CRYPTO_DEV_QCOM_MSM_QCE=y
 CONFIG_CRYPTO_DEV_QCEDEV=y
```

```
diff --git a/bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig b/bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig
index f04574a..c1c2d98 100644
--- a/bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig
+++ b/bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig
@@ -393,6 +393,8 @@ CONFIG_TIMER_STATS=y
 CONFIG_IPC_LOGGING=y
 CONFIG_PANIC_ON_DATA_CORRUPTION=y
 CONFIG_CRYPTO_CRC32C=y
+CONFIG_SAMPLES=y
+CONFIG_SAMPLE_HELLO_MODULE=m
 CONFIG_CRYPTO_DEV_QCRYPTO=y
 CONFIG_CRYPTO_DEV_QCOM_MSM_QCE=y
 CONFIG_CRYPTO_DEV_QCEDEV=y
```

> step 6 编译kernel, 编译kernel_modules, 拷贝新生成的ko文件到bm_rootfs中, 编译rootfs, 烧写验证

本章[示例一 内核模块bma_hello_module]的实际操作示例: [编译](https://asciinema.org/a/238748) [模块上验证](https://asciinema.org/a/238750)

[![asciicast](https://asciinema.org/a/238748.svg)](https://asciinema.org/a/238748)
[![asciicast](https://asciinema.org/a/238750.svg)](https://asciinema.org/a/238750)
