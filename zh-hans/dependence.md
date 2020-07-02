## 依赖

本SDK使用前请检查如下依赖事项是否满足.

### 操作系统

当前SDK的官方验证的操作系统版本为64位的[ubuntu18.04](https://mirrors.huaweicloud.com/ubuntu-releases/bionic/ubuntu-18.04.4-desktop-amd64.iso), 验证如下。其它debian分支的操作系统也许也能正常地进行使用,　但不在官方支持的范围之内.

```shell
frankzhou@jupiter:~$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 18.04.4 LTS
Release:	18.04
Codename:	bionic
frankzhou@jupiter:~$ getconf -a | grep LONG_BIT
LONG_BIT                           64
```
### shell

**请确保/bin/sh指向bash**。如果仍然是使用ubuntu(debian)默认指向的dash, 请执行`sudo ln -sf /bin/bash /bin/sh`进行替换. 验证如下.

```
frankzhou@jupiter:~$ ls -l /bin/sh 
lrwxrwxrwx 1 root root 9 Jun 13 11:49 /bin/sh -> /bin/bash
```
### 操作用户

本SDK需要以普通用户(非root)进行操作,　但需要确保当前用户在sudo组内. sudo只在`make rootfs`目标时会使用.

特别地,  您手中的SDK压缩包请使用**<font color="red">普通用户权限</font>**进行解压, 并且请确保解压目录的绝本路径中不含有空格等特殊字符(即不接受0-9, a-z, A-Z以外的其它字符做为路径)

### menuconfig支持

为了确保您可以正常地执行`make kernel_menuconfig` 与 `make debug_kernel_menuconfig`, 请在您的ubuntu上执行`sudo apt-get install libncurses5-dev`安装ncruse的头文件。

