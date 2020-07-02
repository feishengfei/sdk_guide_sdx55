
## 输入与输出

### 输入

本SDK提供了:

1. 交叉编译工具, host侧(bm_sdk/sysroots/x86_64-linux/)与target侧(bm_sdk/sysroots/mdm9607/)的头文件, 动态库;
2. bootloader(aboot)源代码;
3. kernel源代码;
4. rootfs中的可执行程序,动态链接库,脚本.

本SDK初始不提供:
1. rootfs中可执行程序与动态链接库的源代码.

### 输出

本SDK的输出为:
1. appsboot.mbn (替换到MultiFHLoader/image/appsboot.img)
2. mdm9607-boot.img (替换到MultiFHLoader/image/mdm9607-boot.img)
3. mdm9607-sysfs.ubi (替换到MultiFHLoader/image/mdm9607-sysfs.ubi)
3. mdm9607-data.ubi (替换到MultiFHLoader/image/mdm9607-data.ubi)
