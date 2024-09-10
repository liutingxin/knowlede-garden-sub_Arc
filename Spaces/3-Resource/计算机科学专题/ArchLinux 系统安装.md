---
date created: 2024-09-04
date modified: 2024-09-04
title: ArchLinux 安装
---

```
制作好ArchLinux启动盘后， 使用u盘引导进入系统

进入系统后， 第一件事， 就是设置字体大小， archlinux 为了在不同屏幕下适配，默认情况下，字体会特别小，因此需要set-font ter-132b

然后就需要连接WIFI， 通过 iwctl 进入连接网络的界面
	1. 在连接网络之前，需要获取使用的网卡适配器， 并且扫描附近的网络名称， 然后在进行连接
	2. 直接连接xxx网络：iwctl wlan0 connect xxxx 

接下来，修改下载的软件源，vim /etc/pacman.conf:
	1. 在 normal 模式下 按下 /Color, 搜索这一行内容，取消这一行的注释，用不同颜色显示终端信息
	2. 在 normal 模式下 按下 GG 移动到文件底部，复制并粘贴两行内容：[xxxx] /xxxx/xxx， 然后将中括号里面的内容改成 community
	3. 然后在第二步上的文件路径上， 按 gf 跳转到指定文件， 将 China 源拷贝至文件顶部， 最后在 normal 模式下，按下 :wq 退出即可

接下来使用 lsblk 查看当前的硬盘设备已经分区的一些基本信息， lsblk -f 可查看具体的分区类型， nvme 表示固态硬盘， sda 表示机械硬盘

查看到指定的硬盘设备后， 在固态硬盘上进行分区并创建系统, 使用fdisk /dev/nvme0xx：
	1. 按下 m 可查看帮助信息
	2. 按下 g 可创建一个新的 gpt 的分区， 接下来按下 n 新建分区， 依次输入分区的起始位置， 结束位置等信息
	3. 按下 p 可查看当前的分区信息
	4. 最后按下 w 即可将刚刚的分区信息写入磁盘

结束分区后， 需要对已经分区的硬盘进行格式化， mkfs.fat -F32 /dev/nvmexxx1p1, 这个分区是boot分区，mkfs.ext4  /dev/nvmexxx1p3, 这个分区是根分区，mkfs.ext4 /dev/sda1, 这个分区是home分区

然后就需要将这些分区挂在到我们的磁盘设备上，类似于 乐高积木的拼装 一样，mount /dev/nvme0xxx1p3 /mnt, mount /dev/nvme0xxx1p1 /mnt/boot, mount /dev/sda1 /mnt/home, 一定是先挂载到根分区，然后再挂载其他的分区

使用genfstab -U  /mnt  > /mnt/etc/fstab 文件， 这样系统在开机启动的时候， 就会自动挂载上面这些分区了， 否则还需要我们手动挂载， 但需要注意的是， 如果不自动挂载的话， 系统可能会无法启动，因为 boot 分区存放着我们系统的引导程序，未挂载的话， 系统可能无法找到引导

接下来安装系统的一些包： pacstracp /mnt base linux linux-firmware

arch-chroot /mnt 切换系统到我们的实际系统上去

设置时区， 编码，等信息

然后
```
