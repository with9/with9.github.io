
![  ](/img/SL3Arch.png)

仅个人折腾小记,实际操作有多出需要替换

## 系统迁移

由于资金不足, 购买的SL3容量不足以安装双系统, 因此我的做法是双硬盘双系统, 内置硬盘依然是win10, 外接硬盘为Arch.

### 刻录启动盘

网上教程应该很多, linux下直接dd命令即可, win下推荐rufus工具进行刻录. 有一个神器叫ventory也不错, 可以试试看.

### 分区

通过u盘启动旧电脑,对新磁盘进行分区, 初步分为三个区
- /dev/sdb1 boot分区 2-3G足已 格式 fat32
- /dev/sdb2 root分区 90G 格式ext4
- 剩下都是home分区 格式ext4

通过dd命令将旧电脑的boot分区, root分区, home分区迁移到新磁盘.

## 安装准备工作

由于SL3和原版的linux内核不是特别适配, 直接u盘启动安装你会发现, 内置键盘无法识别, 无法挂载ext4, ntfs分区等问题. 还好已经有大佬们做好了可以直接用的修改内核了.

[项目地址](https://github.com/linux-surface/linux-surface)

linux-surface仓库提前准备好linux-surface内核
- https://pkg.surfacelinux.com/arch/
- https://raw.githubusercontent.com/linux-surface/linux-surface/master/pkg/keys/surface.asc

把linux-surface linux-surface-headers 相关文件保存在boot目录下(因为笔者测试安装过程中仅fat32分区可以正常挂载).

安装结束后推荐添加linux-surface仓库可[参考此链接](https://github.com/linux-surface/linux-surface/wiki/Package-Repositories)

## 安装

需要外置键盘

- 关闭secure boot（先按住F4再开机即可进入Uefi固件设置）

- 挂载磁盘的boot分区

- 安装linux-surface
```shell
    pacman-key --add surface.asc
    pacman-key --finger 56C464BAAC421453
    pacman-key --lsign-key 56C464BAAC421453
    pacman -U linux-surface,linux-surface-headers相关文件
```

- 然后就可以愉快的挂载ext4分区了
  
```
    mount /dev/sda2 /mnt
    mount /dev/sda1 /mnt/boot
    mount /dev/sda3 /mnt/home
```

- 生成新的fstab文件`genfstab -U /mnt  > /mnt/etc/fstab`
  
- 安装grub
  - 务必装在原win10的efi目录下, 否则会无法引导win10
  - chroot `arch-chroot /mnt`
  - 再一次安装linux-surface内核
  - 挂载内置硬盘的efi分区 `mount /dev/nvme0n1p1 /winefi`
  - `grub-install --target=x86_64-efi --efi-directory=/winefi --bootloader-id=grub`
  - 将生成在boot分区的多个img文件及grub目录移动到winefi分区根目录下.
  - 生产grub.cfg文件.`grub-mkconfig -o /winefi/grub/grub.cfg`

接着开机应该就会自动从grub进行引导了, 并且保留有windows boot loader选项方便直接进入windows系统.

## 一些小问题

至此,基本安装已经完成了,但是还存在一些小问题,linux-surface的wiki也大部分给出了解决方法.

### reboot会卡死在开机logo画面.

- 解决方法,grub.cfg文件中加入reboot=pci
- 类似这样linux   /vmlinuz-linux-surface root=UUID=0329c27d-c10f-46da-bcf9-611c721de08e rw reboot=pci

### 待机或睡眠后触摸屏无法使用

- 原因是ipts相关模块挂载问题

- 通过sleep脚本可以解决

- 修改或新建/lib/systemd/system-sleep/sleep文件

```shell
#!/bin/sh
case $1 in
  pre)
    modprobe -r mei
    modprobe -r ipts
  post)
    modprobe mei
    modprobe ipts
esac
```

### 双系统时间不同步

```shell
sudo timedatectl set-local-rtc 1
sudo hwclock --systohc --localtime
```