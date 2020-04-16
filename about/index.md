由于一开始各种诡异的折腾电脑方式,导致我系统的根目录在最后一个分区,并且快满了,无法扩容,因此就打算移动到前面的位置,并且直接迁移到lvm,一劳永逸.

本文是个人用来记录的日志,里面有一些自己踩到的坑,与大家分享一下,讲的不是很全面,如果想要照着迁移的话,最起码要可以照着wiki独立安装archlinux系统

## 提前准备

制作u盘启动盘,命令

```bash
dd if=./archlinux.iso of=/dev/sdb
#将iso文件内容写入u盘,制作启动盘
```
用分区工具先空出合适的空间,推荐gparted,一个很简单的分区工具.
准备就绪,重启从u盘启动系统
## lvm分区
### 创建物理卷
用合适的分区工具对之前留下的空白区域新建分区,分区格式为Linux LVM.
然后在刚刚创建的分区里面创建物理卷(PV)

```shell
pvcreate /dev/sda6 
pvdisplay#可以用该命令列出已经创建的物理卷
```
### 创建卷组
在刚刚创建的物理卷上创建卷组(VG)

```shell
vgcreate vg00 /dev/sda6 #此命令利用sda6分区创建一个名称为vg00的卷组
vgextend vg00 /dev/sda5 #此命令可以将vg00卷组扩展到其他物理卷
vgdisplay #列出已经创建的卷组
vgcreate vg00 /dev/sda6 /dev/sda5 #也可以用此命令直接在多个物理卷的一个卷组
```

### 创建逻辑卷(LV)

```shell
lvcreate -L <卷大小> <"卷组名> -n <卷名> #命令格式如此
eg. lvcreate -L 60G vg00 -n lvroot #在vg00卷组中创建名称为lvroot的逻辑卷,大小为60G
lvcreate -l +100%FREE vg00 -n lvhome #将vg00卷组剩余容量全部给lvhome逻辑卷.
```
### 系统迁移
接着将原root分区的内容直接写入新的分区,dd命令是直接以非常简单的方式把输入端的内容完全写到复制端,如果是分区的话,那么是把整个分区内容写入输入端,而不是文件,因此新创建的root逻辑卷必须要比原root分区大,不只是比已用空间大.比如原root分区大小为60G,使用不到10G,依然会读取60G的内容写入新的逻辑卷.

```shell
dd if = /dev/sda7 of=/dev/vg00/lvroot #将原root分区内容写入新的root分区
```

此命令过后lvroot分区内容会和旧root分区一模一样,挂载后运行``df -h ``命令,你会发现和旧分区大小一模一样,无论我们分了多少空间给新的root分区,文件系统都会识别为旧root分区的大小,我们用resize2fs命令来调整文件系统的大小,使得其达到逻辑卷的大小.

```shell
resize2fs /dev/vg00/lvroot
```

挂载root分区到/mnt boot分区到/mnt/boot,并从u盘系统切换到新的系统

```shell
mount /dev/vg00/lvroot /mnt
mount /dev/sda1 /mnt/boot
arch-chroot /mnt
```
### 配置mkinitcpio
修改mkinitcpio文件,因为你的根文件系统基于LVM，需要启用适当的[mkinitcpio](https://wiki.archlinux.org/index.php/Mkinitcpio)钩子，否则系统可能无法启动。

修改配置文件,添加systemd和sd-lvm2两个钩子

```shell
/etc/mkinitcpio.conf
HOOKS=(base *systemd* ... block *sd-lvm2* filesystems)

mkinitcpio -P #重新生成initramfs
```

修改fstab文件,将根目录挂载到新分区.

```shell
/etc/fstab
# /dev/mapper/vg00-lvroot UUID=299cd25f-28f7-4a5f-bea8-0afa2e017f67
/dev/mapper/vg00-lvroot /               ext4            rw,relatime     0 1
```
### 修改引导文件
重新生成grub引导文件

```shell
grub-mkconfig -o /boot/grub/grub.cfg
```

由于lvm下的thin_check问题,导致默认的10s rootdelay会不够,还没有找到新的root分区前就会报错,我们修改rootdelay ,在这行的最后添加``rootdelay=60``

```shell
/boot/grub/grub.cfg
echo    'Loading Linux linux ...'
linux   /vmlinuz-linux root=/dev/mapper/vg00-lvroot rw  quiet *rootdelay=60*
```
推出chroot环境,重启就行了,如果电脑上有为win系统的话,此时是无法找到的,并且还会找到旧的root分区系统,可以格式化也可以留着并且新建boot目录(注意,不是作为grub引导的分区),并安装内核,重启后再次运行``grub-mkconfig``命令

[^1]: https://wiki.archlinux.org/index.php/LVM
