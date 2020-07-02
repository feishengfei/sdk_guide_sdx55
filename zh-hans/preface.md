# 序言

本SDK旨在提供客户SDX55(TW-Q5500G-GL|TW-Q5510M-GL)系列模组的交叉编译工具链、kernel内核源码、部分驱动源码、以及rootfs与OTA升级底包, 以供客户进行自身应用/驱动/OTA升级的二次开发。

本SDK说明文档主要介绍了
* SDK可以用来做哪些事情;
* SDK使用的前置依赖;
* 如何初始化工具链;
* 如何进行内核配置编译;
* 如何打包制作rootfs(system)与data影像;
* 如何进行模组通用API的编译;
* 如何使用SDK进行应用程序的开发(GNU Makefile/cmake各一例);
* 如何使编写的应用随开机自行启动;
* 如何制作OTA升级包(TBD)

### 输入

本SDK提供了:

* 交叉编译工具, host侧(**sdk/sysroots/x86_64-oesdk-linux/目录**)与target侧(**sdk/sysroots/armv7at2hf-neon-oe-linux-gnueabi/目录**)的头文件, 动态库;
* kernel源代码(**sdk_kernel/目录**);
* 部分驱动源代码(**ext_driver/目录**);
* 模组通用API接口的头文件、动态库、以及编写示例(**ext_sdk/目录**);
* system只读分区的可执行程序，动态链接库，脚本等原始档(**sdk_rootfs/目录**，用于生成system只读分区影像);
* data读写分区的原始档(**sdk_usrfs/目录**，用于生成data读写分区影像);
* 用于OTA升级的原始档案资料(**sdxprairie-ota-target-image-ubi/目录**，配合release-tools来生成recovery升级用到的update.zip压缩包)

本SDK不提供:

* 模组通用API接口的源代码;
* system只读分区中可执行程序与动态链接库的源代码;
* data读写分区的可执行程序的源代码;

### 输出

本SDK最终可以生成并使用的影像为

* **sdxprairie-boot.img** 内核影像
* **sdxprairie-data.ubi** data数据分区(可读可写)影像
* **sdxprairie-sysfs.ubi** system分区(只读)影像

以上生成的文件位于sdk根目录下的output文件夹中。


以上生成的影像可供替换到windows升级工具**FirmwareUpgrader(T&W).exe**同级的image文件夹中进行模块的一键升级。

## 注意

* 对于非AP应用处理器核心的影像，仅随升级工具提供，SDK并不能开发这些影像;
* 分区表信息随升级工具提供，SDK并不能配置修改分区表信息。
