# alsa驱动架构介绍
* [参考地址](https://blog.csdn.net/sanmaoljh/article/category/6387692)

## 01 概念简介

```sh
  声卡芯片：struct snd_card
  对于声卡芯片来说，此声卡的每一个功能都是一个逻辑设备：snd_device
```

<div>什么是上下文</div>

```sh
  所谓上下文，就是在创建某一个对象时，为这个对象创建一个全局变量（或者是堆变量）来保存这个对象的信息。也就是说上下文就是一个全局变量（也叫作环境变量）
```


## 02 声卡的功能（逻辑设备）

```sh
  cd /dev/snd/
  [rootAplex snd]# ls -lh
  total 0
  crw-rw----    1 root     root      116,   0 Jan  1  1970 controlC0
  crw-rw----    1 root     root      116,  24 Jan  1  1970 pcmC0D0c
  crw-rw----    1 root     root      116,  16 Jan  1  1970 pcmC0D0p
  crw-rw----    1 root     root      116,  33 Jan  1  1970 timer
  [rootAplex snd]#

其中，C0D0代表的是声卡0中的设备0，pcmC0D0c最后一个c代表capture，pcmC0D0p最后一个p代表playback，这些都是alsa-driver中的命名规则。
由上面的文件可知，本声卡具有上面四个功能
```

## 03 ASOC驱动框架简介

```sh
  1. ASoC驱动框架被分为Machine，Platform（soc）和Codec三大部件
    Platform:soc层硬件无关接口层（提供抽象等API）
    Codec：声卡层硬件无关接口层（提供抽象等API）
    Machine：具体的产品，比如sbc_7109_455,通过CodecAPI和socapi来绑定soc、codec等实现一个具体的声卡驱动。

  2. 声卡驱动的运行条件：
    声卡对应的soc控制器驱动
    具体的声卡驱动
```
