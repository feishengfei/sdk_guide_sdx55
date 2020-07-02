
## 目录结构

```
frankzhou@jupiter:~/opensdk_20200702$ tree -L 1
.
├── ext_driver      /*部分驱动源码*/
├── ext_sdk         /*API与实例*/
├── Makefile        /*根的Makefile，用于编译各个影像*/
├── output          /*镜像输出目录*/
├── sdk             /*工具链相关*/
├── sdk_kernel      /*kernel源代码*/
├── sdk_rootfs      /*system只读分区(rootfs)文件系统(仅可执行程序/动态库/脚本，无源码)*/
├── sdk_usrfs       /*data读写分区文件系统(仅可执行程序/动态库/脚本，无源码)
└── sdxprairie-ota-target-image-ubi		/*OTA升级相关目录*/
```
