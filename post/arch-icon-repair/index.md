
~~**置顶一下最后说的话:Arch大法好!**~~

系统好久没有滚一下了,于是输入了``pacman -Syu``滚了一下系统,有一些小冲突,没有特别在意的替换了,然后开机一开始一切顺利,就是图标出现了各种的问题,好多应用图标发生了丢失,变成了这个样子.

![  ](/img/arch-repair/1.png)

一些应用是好的,一些丢失了,一开始我以为是因为图标包的问题,然后就下了几个其他的图标包,问题还是存在,而且应用完全没有用图标包,而是用了hicolor的原始图标,甚至有些图标包一使用桌面直接崩溃..

首先,我以为是用户配置的问题,就把.config文件夹重新命名了下,发现还是一样的问题,而且切换某些图标包后,无法进入桌面,终端修改后才可以进入,明显不是简单的用户配置的问题.

我还试着用了下之前备份的更目录的icons覆盖这个目录,发现也没有用.
没有办法,我先试试看编辑.desktop可不可以修复单独的一个应用问题..,没有找到网易云的图标,随便网上下了个,似乎成功了

![  ](/img/arch-repair/2.png)

![  ](/img/arch-repair/3.png)![  ](/img/arch-repair/4.png)

就在我屈服于此,打算一个一个把常用的应用都手动修复下,在修复tim的时候,通过cuttlefish找到了电脑里面存在的svg图标(顺便吐槽一下这个图标预览器自己图标也丢失了.......),并没有像之前的网易云那样直接被修复.

![  ](/img/arch-repair/7.png)

当时很困惑,我顺着打开了``/usr/share/icons/hicolor``文件夹,发现凡是png格式的应用图标都可以显示,而svg格式的应用图标都无法正常显示
![  ](/img/arch-repair/8.png)

到这里,我感觉我终于发现了突破口,毫无疑问,一定和svg有所关系,于是网上寻找svg有关的包,发现有一个librsvg,猜测可能是系统更新的时候这个包因为冲突没有安装什么的,先直接pacman安装看看,结果居然它已经存在了,然后站在aur上中找到了librsvg-git的包,重新编译安装了下,注销一下重新登录,一切都修复啦..

最后截一张修复好了的图.

![ ](/img/arch-repair/9.png)