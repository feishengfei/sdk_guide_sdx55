## 编译

### 初始化环境变量(交叉工具链)

执行`source bm_sdk/environment-setup-armv7a-vfp-neon-oe-linux-gnueabi`以进行环境变量初始化工具.

执行`which $CC`查看是否有输出,　如果有则表明工具链环境变量等内容已经初始化完毕.

如果您需要对cross-toolchain的初始化脚本进行更改， 请在`bm_sdk/environment-setup-armv7a-vfp-neon-oe-linux-gnueabi`中进行， 比如扩展CFLAGS， CXXFLAGS， LDFLAGS等等。

###  编译kernel

####  kernel配置

* `make kernel_menuconfig` 选择面向最终用户(release版)的配置文件为模板， 可以自行配置；模版文件位于'bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig';
* `make debug_kernel_menuconfig`， 选择打开内核调试(debug版)的配置文件为模板， 可以自行配置；模版文件位于'bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig';
* `make kernel_defconfig` 选择面向最终用户的配置文件为模板；模版文件位于'bm_kernel/msm-3.18/arch/arm/configs/mdm9607-perf_defconfig'.
* `make debug_kernel_defconfig` 选择打开内核调试(debug版)的配置文件为模板；模版文件位于'bm_kernel/msm-3.18/arch/arm/configs/mdm9607_defconfig'.


**注意**

上述模板最终会替换到'bm_kernel/msm-3.18/build/.config'中.
请确保已经正确安装libncurse5-dev以进行menuconfig, 如需安装请执行`sudo apt-get install libncurse5-dev`.

####  kernel编译

执行`make kernel`进行内核编译。生成的kernel影像位于output文件夹中(output/mdm9607-boot.img)。
执行`make kernel/clean`清除内核生成的img文件与中间文件。

**注意**

`make kernel`之后，会自动使用mdm9607-boot.img的内核影像更新ota文件夹下BOOTABLE_IMAGES中的recovery.img与boot.img。

####  kernel模块编译

执行`make kernel_module`进行内核模块编译, 并同步更新到bm_rootfs的对应目录中。
该步操作之后请重新`make rootfs`以保证生成的ko模块更新到rootfs中。
目前使用debug_kernel_menuconfig配置的内核, 需要先进行`make kernel`，再执行`make kernel_module`，由于部分debug的宏激活的函数定义位于全局符号表中。

###  编译rootfs

执行`make rootfs`进行rootfs/usrfs的影像文件生成.

**注意**

* 请在制作rootfs前确保您的应用程序/动态库/配置/脚本都已经正确地更新到bm_rootfs相应的文件夹中.
* 本步操作要求用户拥有sudo权限。
* `make rootfs`时会先根据当前UTC时间更新/etc/timestamp文件
* `make rootfs`时会先根据当前本地时间更新/etc/version文件

###  编译example

bm_ext_sdk/example下有一些测试程序, 可以在该目录下直接make来编译相关的测试程序.
这些测试程序可以手动拷贝到bm_rootfs中并`make rootfs`将生成的测试程序打包到mdm9607-sysfs.ubi中, 或通过`adb push`将应用推到模块的可写分区中进行试验.

###  烧录与验证

```
 output/appsboot.mbn             ->       MultiFHLoader/image/appsboot.img
 output/mdm9607-boot.img      ->       MultiFHLoader/image/mdm9607-boot.img
 output/mdm9607-sysfs.ubi     ->       MultiFHLoader/image/mdm9607-sysfs.ubi
 output/mdm9607-data.ubi      ->       MultiFHLoader/image/mdm9607-data.ubi
```

请按照上述重命名对应关系进行一键(多路)升级工具MultiFHLoader/image下对应文件的替换.
验证kernel(mdm9607-boot.img)是否生效的方法是进入板子上查看`cat /proc/version`是否已经显示您编译主机上的用户名与主机名.
验证rootfs的方法是查看`/etc/timestamp`与`/etc/version`中记录的编译的时间戳.

本章[编译]的实际操作示例[如下](https://asciinema.org/a/238577)

[![asciicast](https://asciinema.org/a/238577.svg)](https://asciinema.org/a/238577)
