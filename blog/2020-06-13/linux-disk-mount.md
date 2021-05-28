# 树莓派烧卡
* 磁盘管理基础知识
  * 磁盘管理
  * 分区
  * 挂载点
* 烧卡
* 如何加载 extfs4 格式

## 磁盘管理基础知识
### 设备驱动
* 面向包的网络设备驱动
* 面向块的存储设备驱动
* 面向字节的字符设备驱动

![](/pic/linux-device.png)

### 查看加载的磁盘（mac）
`diskutil list ` 

``` bash
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *63.9 GB    disk2
   1:             Windows_FAT_32 boot                    268.4 MB   disk2s1
   2:                      Linux                         63.6 GB    disk2s2
```

### 磁盘到分区
`diskutil eraseDisk FAT32  BOOT /dev/disk2`


## 烧卡
`sudo dd if=~/Desktop/raspberrypi.dmg of=/dev/disk2`

## 如何加载extfs格式在mac
macos 默认不支持 extfs的读写~~  
* [osx-fuse](https://osxfuse.github.io/) 来增加 macos 支持的分区格式
* [ext4fuse](https://github.com/gerard/ext4fuse) 为 macos 增加ext4 分区格式的读
* [fuse-ext2](https://github.com/alperakcan/fuse-ext2) 为macos 增加 ext 分区格式的写

安装说明：
1. `brew cask install osxfuse`
1. `brew install ext4fuse`
1. 依照 [fuse-ext2-安装说明](https://github.com/alperakcan/fuse-ext2/blob/master/README.md#macos) 在mac安装ex2
1. 挂载   
   ``` bash
   Usage: fuse-ext2 <device|image_file> <mount_point> [-o option[,...]]  
   Options:  ro, rw+, force, allow_other
   Example:  fuse-ext2 /dev/sda1 /mnt/sda1
   ```
1. 卸载
   ``` bash
  sudo umount -f
   ```
