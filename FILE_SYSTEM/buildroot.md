# buildroot的配置与使用
## 00 目录
* [01 buildroot环境搭建](#jump01)
* [02 buildroot扩展](#jump02)

## <span id="jump01">01 buildroot环境搭建
[buildroot官方文档](https://buildroot.org/downloads/manual/manual.html)
* 操作系统：Ubuntu 14.04

```sh
  1. 如果交叉编译工具链是32位，需要下载32位的依赖库
    sudo apt-get install   libc6:i386   libstdc++6:i386   libncurses5:i386 zlib1g:i386

  2. 安装buildroot
    sudo apt-get update –y
    wget  https://buildroot.org/downloads/buildroot-2017.02.3.tar.gz
    tar xvf buildroot-2017.02.3.tar.gz
    sudo apt-get install texinfo  -y
    sudo apt-get install libncurses5-dev -y
    sudo add-apt-repository ppa:webupd8team/java  -y
    sudo apt-get update
    sudo apt-get install oracle-java8-installer -y
    sudo apt-get install oracle-java8-set-default -y
  3. buildroot的下载地址
    https://buildroot.org/downloads/
```
* 根据arm架构配置buildroot

```sh
  1. 打开buildroot的配置界面
    make menuconfig
    make busybox-menuconfig
  2. 界面配置如下图所示。
```
* Buildroot 简介

![safe](imagebuildroot/图片1.png)

* 配置开发板架构

![safe](imagebuildroot/图片2.png)

* 配置外部交叉编译工具链

![safe](imagebuildroot/图片3.png)

* 配置 根文件系统选项

![safe](imagebuildroot/图片4.png)

* 根文件系统压缩包的压缩方式

![safe](imagebuildroot/图片5.png)

# <span id="jump02">02 buildroot 扩展
* 2.1 添加自己的软件包
  * 1. 我有一个软件包（uart1）

  ![safe](imagebuildroot/图片11.png)

  * 2. 将我的软件包添加到buildroot中

  ```sh
    1. 在buildroot中创建软件包的配置信息
      在buildroot/package/ 下执行 mkdir  uart1 ，创建uart1 的文件夹
      在uart1 文件夹下面建立两个配置文件Config.in 和 uart1.mk

      在uart1.mk文件中添加如下内容
          UART1_SOURCE = uart1.tar //软件包名
          UART1_SITE = file:///home/aplex //软件包所在的位置
          define UART1_BUILD_CMDS     //编译命令
                  $(MAKE)   -C $(@D)    //调到我的uart1软件包里面执行uart1里面的Mkaefile文件
          endef
          define UART1_INSTALL_TARGET_CMDS    // 安装命令
                  $(INSTALL) -D -m 755 $(@D)/lws_uart  $(TARGET_DIR)/bin
          endef
          $(eval $(generic-package))

      在Config.in 文件中添加如下内容
          config BR2_PACKAGE_UART1
                     bool"uart1"
          default y
                     select BR2_PACKAGE_LIBWEBSOCKETS //本软件所依赖的库
                     select BR2_PACKAGE_JSONCPP
                     help
                       demo app to show  

      2. 将uart1注册到buildroot系统中
          在package/Config.in 中加入 source "package/demo_app/Config.in" ，以便将uart1 的配置文件包含到Buildroot的package中来管理
      3. 参考案列：
        Config.in文件的仿照案例，在buildroot-2017.02.3/package目录下随便找一篇文档即可参照。
        uart1.mk文件的仿照案例：
          1 generic-package ：参考文件package/vsftpd/vsftpd.mk：
          2：autotools-package ： grep "autotools-package"  package/  -r
          3：cmake-package ：grep "cmake-package"  package/  -r
          4：python-package：grep python-package""  package/  -r
        网址参考介绍：
        TSLIB_SITE = https://github.com/kergoth/tslib/releases/download/$(TSLIB_VERSION)
        UART1_SITE = file:///home/aplex 或者 /home/aplex
        LIBFOO_SITE = http://www.foosoftware.org/download
        使用git网址：将未压缩的软件源码上传到githup上，创建发布版本，即可在buildroot中使用git下载源码包，如下图
  ```

![git](imagebuildroot/图片13.png)

![git](imagebuildroot/图片14.png)

* 2.2 合并新老系统[参考地址](https://github.com/ZengjfOS/Buildroot/tree/master/customize/rootfs/etc)

![git](imagebuildroot/图片9.png)

* 2.3 编译自己的内核

![git](imagebuildroot/图片10.png)

```sh
注意上图有错误：
(am335x-cmi_at751) Device Tree Source file names （不要带后缀名 .dts）
```
* 2.5编译3Dqt

```sh
  使用buildroot编译QT需要三步
  1. 选择内核
  2. 选择ti-sgx-km（BR2_PACKAGE_TI_SGX_KM）
  3. 选择QT
```
* 2.5 编译自己的uboot
  * pass
* 2.6 添加githup软件包
  * [第一种方式](https://buildroot.org/downloads/manual/manual.html)

  ![git](imagebuildroot/图片6.png)

  ![git](imagebuildroot/图片7.png)

  * 第二种方式
  
  ![git](imagebuildroot/图片8.png)
