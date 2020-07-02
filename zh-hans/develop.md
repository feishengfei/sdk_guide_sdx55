
## 开发者须知

作为开发者, 需要关注一下目前host侧与target侧的一些工具, 动态库, 头文件的存放位置及其含义, 重点关重例表如下. 另外, 可以查看bm_sdk/environment-setup-armv7a-vfp-neon-oe-linux-gnueabi中export到当前会话中起作用的环境变量.

* host侧(x86)

|路径|含义|
|:-|:-|
|bm_sdk/sysroots/x86_64-linux/usr/bin/arm-oe-linux-gnueabi/|cross toolchain的位置|

* target侧(arm)

|路径|含义|
|:-|:-|
|bm_sdk/sysroots/mdm9607/usr/include/|模块上各个动态库的头文件|
|bm_sdk/sysroots/mdm9607/usr/lib/|模块上各个动态库的库文件|
|bm_sdk/sysroots/mdm9607/usr/lib/pkgconfig/|目前板子上各个应用包的版本信息, 可以配合bm_sdk/sysroots/x86_64-linux/usr/bin/pkg-config使用|

