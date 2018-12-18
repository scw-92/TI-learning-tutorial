# 制作UBIFS文件系统
## 01 UBI文件系统简要说明
* ubifs文件系统是工作在mtd分区之上的，所以对于此系统来说，ubifs文件系统在mtd4分区上。

##  02 定制480兆的ubi文件系统
* 01 在设备树文件中预留好合适的mtd分区（mtd4_siz = 495M ）
  ```sh
  以mtd4分区为例来制作ubi文件系统：
  partition@4 {
        label = "NAND.rootfs.ubi";
        reg = <0x01780000 0x20680000>;
        # 0x01780000 nandflash 的物理地址（本mtd分区的起始地址）
        # 0x1e000000 本mtd分区的大小，但是是字节(byte)
        #（0x1e000000 - 0x01780000） / 1024 /1024 = 456M
    };

  ```
* 02 确定ubifs文件系统所擦除的nand的大小（ubinize.cfg文件内容）: cat ubinize.cfg

```sh
[ubifs]
mode=ubi
image=ubifs.img
vol_id=0
vol_size=480MiB
vol_type=dynamic
vol_name=rootfs
vol_flags=autoresize

# vol_size=480MiB 的来龙去脉:
  1. 从ubi文件系统的角度来说，ubi_size = （OOB + 有效磁盘数据）* 磁盘的块数
                                     = （4KB + 124KB）* 3840
                                     =  128*3840/1024 M
                                     =  480M
  2. 从内核（mtd）的角度来说，ubi文件系统是mtd分区的有效数据也就是：mtd4_size = OOB + ubi_size
                                                             = OOB + 480M
                                                             = 495M

```
* 03 cat  mkubiimg.sh

```sh
#sudo ./mkfs.ubifs  -q -r rootfs -m 2048 -e 126976 -c 2047 -o  ubifs.img -F
sudo mkfs.ubifs  -q -r rootfs_release -m 2048 -e 126976 -c 3840 -o  ubifs.img -F

echo mkfs.ubifs over!
sudo ubinize -o ubi.img -m 2048 -p 128KiB ubinize.cfg
echo ubinize over!

sync

sudo cp ubi.img ~/image -rf

sudo rm ubifs.img  ubi.img  -rf
sync
echo make file system ok !

```
* ubi与mtd分区大小的对应关系
```sh
  1. 从MTD的角度来说：
    ubi文件系统是mtd分区的有效数据区
     mtd4_siz = ubi_size + OOB
  2. 从ubi文件系统的角度来说：
    ubi_size = OOB + 有效数据区
```
