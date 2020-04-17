
最近打算把封印了快一年的midi键盘拿出来练下,拿出来才发现,我的系统换成了debian,还不知道要怎么链接midi键盘,果断网上搜索 关键词midi键盘 linux系统,,果然没有想freepiano这样子的傻瓜软件....结果看到一些教程,但是都不详细,就是说qsynth+jack+timidity+vmpk这几个配合起来,先不管了,首先先apt安装这些,发现大部分都是预装上的...

下面讲一下我折腾了快两天得出来的经验教程,首先我们打开qjackctl软件,应该是一个控制音频接口的软件吧,一些教程讲这里直接点start,再打开vmpk就可以弹琴了,这是不行的,(我试过了,jack会报错..)

我们先什都不用管,接着打开vmpk,编辑里面链接,midi输入为我们的midi键盘,然后上面的允许midi thru转发到midi输入点上钩钩,然后确认

![ ](/img/midi/1.png)

接着我们再回到qjckctl,点击connect,到标签的ALSA下面,将midi键盘的输入接到vmpk的输出(**一定要在qsynth打开前接入,要不然会无法链接过去,原理不太清楚,我被这个坑了好久**).

![ ](/img/midi/2.png)

 接着打开qsynth,点击setup,soundfonts(音源)打开我们下载好的音源,我的是freepats上面的kawai钢琴音源(要求sf2格式的,诸如sfz这些格式好像不可以),

 然后保存设置,接着回到qjackctl,点击start,然后点击connect,标签切换到Audio,将fluidsynth链接到system的输入,再切换到ALSA,发现右边的输入接口多了一个fluidsynth,将它于vmpk的输出接口连在一起,

![ ](/img/midi/4.png)

![ ](/img/midi/5.png)

大功告成,~~可以开始练习啦~~.附全家福(雾)

![ ](/img/midi/6.png)