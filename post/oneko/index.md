
![Peek 2021-11-10 15-48](/img/oneko/Peek%202021-11-10%2015-48.gif)

## oneko介绍

最近在aur上看到一个有些时间了的桌宠项目[Oneko](https://github.com/tie/oneko),下过来编译运行后会有一直小猫追着你的鼠标跑,很可爱


除了小猫以外还有很多其他的角色,追击模式也有很多种,可以让一直跟着或者呆在窗口最上面,具体使用请查阅手册`man oneko`

安装也很简单,直接aur上安装就行`paru -S oneko`

## 调整大小
但是有个大问题,就是它太小了,对于现在动不动就是2k,4k的显示器来说,默认的32x32pixe的图片实在是太小了,有没有什么办法让它变得更大一点呢,下面分享一下我折腾的过程.

首先我们要获得软件源代码,可以直接去官网下载,修改源代码后进行编译安装. 不过makepkg提供了一个更加简单的方式,可以直接修改编译后将其打包为pacman的软件包.

### 简单介绍makepkg的用法

makepkg是Arch系统的一个命令工具,用于软件包自动编译,运行时需要有一个PKGBUILD文件,PKGBUILD本质是一个shell脚本,记录软件包的一些信息和编译步骤.
```shell
makepkg -Si #编译并安装
makepkg -o #仅解压缩源代码
makepkg -e #仅编译并打包(不解压和下载源代码)
```

遇到需要修改源代码的软件,我一般的流程是先解压,修改后编译并打包,测试没有问题后再利用pacman进行安装.

### 源代码获取并修改
如果是aur安装的话,源代码一般已经下载到本地了.对于paru来说,下载的编译脚本是在`$HOME/.cache/paru/clone/`下,我们进入oneko的地址,底下应该是一个PKGBUILD文件和源码压缩包,首先`makepkg -o`进行解压,默认情况下,源代码会解压到src文件夹下,观察src下有一个`oneko.h`头文件,根据经验来看,这个文件里应该会有图片尺寸等参数,果然让我找到了类似的定义

```c
#define BITMAP_WIDTH        32

#define BITMAP_HEIGHT       32
```

先把其中的32都改为50,然后回到PKGBUILD所在目录,运行`makepkg -ef`进行编译,确实变大了,但是图片出大问题了.

![image-20211110160219235](/img/oneko/image-20211110160219235.png)

这一坨是啥😂.. 看来单纯的改定义是不行的,我们还需要修改图片的尺寸. 图片在`bitmap` 和`bitmasks`两个文件夹下面,都是32x32的xbm格式图片,用vim打开的话可以看到类似这样的内容,关于xbm格式的详细内容可以参考[这篇文章](https://zhuanlan.zhihu.com/p/358945096),大致上一个十六进制字节代表一个2x2的方格的排列情况(2^4).

```c
#define up1_bsd_mask_width 32
#define up1_bsd_mask_height 32
static char up1_bsd_mask_bits[] = {
 0x00,0x06,0x30,0x00,0x00,0x07,0x70,0x00,0x80,0x03,0xe0,0x00,0xc0,0x03,0xe0,
 0x01,0xc0,0xfb,0xef,0x01,0xc0,0xff,0xff,0x01,0xc0,0xff,0xff,0x01,0xc0,0xff,
 0xff,0x01,0x80,0xff,0xff,0x00,0x80,0xff,0xff,0x00,0x00,0xff,0x7f,0x00,0x00,
 0xff,0x7f,0x00,0x00,0xff,0x7f,0x00,0x00,0xff,0x7f,0x00,0x00,0xfe,0x3f,0x00,
 0x00,0xfc,0x1f,0x00,0x00,0xfc,0x1f,0x00,0x00,0xfe,0x3f,0x00,0x00,0xff,0x7f,
 0x00,0x00,0xff,0x7f,0x00,0x80,0xff,0x7f,0x00,0x80,0xff,0xbf,0x00,0x80,0xff,
 0x3f,0x01,0x00,0xff,0x3f,0x02,0x00,0xfc,0x3f,0x00,0x00,0xfc,0x3f,0x00,0x00,
 0xf8,0x7f,0x00,0x00,0x40,0x7f,0x00,0x00,0x40,0x3e,0x00,0x00,0x28,0x00,0x00,
 0x00,0x18,0x00,0x00,0x00,0x38,0x00,0x00};
```

可以自己编写脚本进行缩放,也可以利用GIMP等图片软件进行编辑后覆盖,我选择后者.选择缩放到50像素后覆盖原文件.

![image-20211110160754713](/img/oneko/image-20211110160754713.png)

### 利用GIMP的BIMP插件进行批处理

另一个问题来了,图片这么多,难道要我们手动一个一个的进行修改吗,一番搜索后发现一个很不错的GIMP插件,[BIMP](https://alessandrofrancesconi.it/projects/bimp/),可以批处理一些简单的动作,AUR上也有这个包,安装后就可以在文件那一栏运行该插件.

![image-20211110161404357](/img/oneko/image-20211110161404357.png)

然后把导出的图片覆盖掉原来的图片,继续编译,不出意外,编译报错了..

![image-20211110161637885](/img/oneko/image-20211110161637885.png)

### 调整导出图片的格式

分析一下导出的图片,发现格式定义上和源文件有出入,丢失了部分内容,还好丢失的部分内容可以从文件名上获得,编写一个小的fish脚本利用sed来处理这一步

```c
//导出后的格式
#define _width 50
#define _height 50
#define _x_hot 0
#define _y_hot 0
static unsigned char _bits[] = {.....};
___________________________________________
//源文件内容
#define up2_tomoyo_mask_width 32
#define up2_tomoyo_mask_height 32
static unsigned char up2_tomoyo_mask_bits[] = {......}
```

```shell
for i in (ls *.xbm)    >
    set filename (basename $i .xbm)
    sed -i "s/define\ _height/define\ $filename\_height/g" $i
    sed -i "s/define\ _width/define\ $filename\_width/g" $i
    sed -i '/_x_hot/d' $i
    sed -i '/_y_hot/d' $i
    sed -i "s/char\ _bits/char\ $filename\_bits/g" $i
end
```

接着再进行编译`makepkg -ef` 并安装`pacman -U xxxxx.zst`就可以开心的玩耍啦.
