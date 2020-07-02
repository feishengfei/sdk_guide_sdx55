# 简介

本SDK旨在提供客户SDX55(TW-Q5500G-GL|TW-Q5510M-GL)系列模组的交叉编译工具链、kernel内核源码、部分驱动源码、以及rootfs与OTA升级底包, 以供客户进行自身应用/驱动的二次开发。

本SDK最终可以生成并使用的影像为sdx-boot.img, mdm9607-data.ubi以及mdm9607-sysfs.ubi, 以上生成的文件位于sdk工具链根目录下的output文件夹中. 以上生成的影像可供替换到多路升级工具(MultiFHLoader)的image文件夹中进行模块的一键升级.
