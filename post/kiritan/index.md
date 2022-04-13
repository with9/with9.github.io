![ 95633925 ](/img/kiritann/bg.png)

> https://www.pixiv.net/artworks/95633925

## 简介

最近b站一直收到一个神秘歌姬视频的推送,歌曲多是各个地方的军歌(

{{< bilibili "BV1eT4y1e7S6" >}}


声音非常好听,**人设也蛮可爱的**,打算了解一下相关内容,身份好像蛮复杂的,可以去[萌娘百科]()进行详细了解.其中有个叫[NEUTRINO](https://zh.moegirl.org.cn/NEUTRINO)的项目,某日本神秘程序员开发,我们只要丢谱子过去,它就会通过神经网络进行调教并返回音频文件,很有意思,很多视频上的AIきりたん标签就是用的这个引擎制作的(有些是cevio AI制作的~~天国的莎莎拉~~)

## 安装

进入官网点击download进行下载,会跳到谷歌云盘(记得科学上网),我是linux系统,因此下载NEUTRINO-Online,音源的话在Singer Library文件夹内,选择喜欢的歌姬下载即可,这里选择`東北きりたん（NEUTRINO-Library)`

接着我们测试一下,压缩文件,进入NEUTRINO-Online, 运行run.sh脚本,如果顺利的话,就会进行调教,生成的音频文件,文件在output里面. 一开始的测试文件是sample1.

## 简单的修改

测试顺利,接下来就是设定歌姬并让她唱其他歌曲啦,将下载的歌姬模型文件丢入`model`文件夹下
基本的设置都在run.sh文件里面,打开可以发现定义了一些简单的变量,基础的话,我们只用修改这些变量就可以啦,首先是乐谱,然后是歌姬.

接着我们试试自定义乐谱,简单地用musescore制作下乐谱,导出为musicxml格式, 记得加上换气符号,要不然真的会一口气唱下去然后声音逐渐沙哑,非常真实(

![image-20220409124541099](/img/kiritann/image-20220409124541099.png)

修改一下内容

```shell
# Project settings
BASENAME=我会自己上厕所#乐谱名称
NumThreads=3

# musicXML_to_label
SUFFIX=musicxml #乐谱后缀

# NEUTRINO
ModelDir=KIRITAN #歌姬声源文件夹
StyleShift=0
```

然后运行即可,(PS 后面歌词有点问题,拗音还要研究下,就截取前面15秒吧.)

{{< audio src="/audio/out.wav" class="something" >}}