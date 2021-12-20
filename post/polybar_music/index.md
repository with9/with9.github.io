{{% music id="28918133" auto="0" %}}

## polybar简单介绍

[polybar](https://polybar.github.io/)是一个可以简单进行自定义的状态栏工具,通过各模块进行组合,可定制性非常强.这是我东拼西凑的一个样子😁

![Screenshot_20211216_205158](/img/polybar_music/Screenshot_20211216_205158.png)

![image-20211216205419967](/img/polybar_music/image-20211216205419967.png)

[桌面壁纸](https://www.pixiv.net/artworks/89075761)

今天读polybar wiki的时候,发现有一个模块是script,可以自定义并执行一些脚本,好像很好玩,官方仓库也有很多的[用户贡献的script例子](https://github.com/polybar/polybar-scripts),其中有很多是和音乐有关的.

![image-20211216205720202](/img/polybar_music/image-20211216205720202.png)

## 初步尝试

但是以spotify为主,对我用处不是很大,就想自己写一个用来显示网易云正在播放的音乐的script模块,我发现`wmctrl`可以获取到窗口的标题,而网易云音乐的标题正好就是当前播放的曲目.

![image-20211216210039834](/img/polybar_music/image-20211216210039834.png)

通过`grep`是把这行获取到了,但是前面有很多无用信息,一开始尝试`cut`,但效果很奇怪,网上搜索一番,有一个关于如何用`wmctrl`获取标题的[讨论](https://unix.stackexchange.com/questions/342404/getting-just-the-application-name-from-wmctrl-l),各种很奇怪的方式😂,为什么`wmctrl`不可以提供一个格式化输出的命令呢

![image-20211216210814223](/img/polybar_music/image-20211216210814223.png)

一番尝试发现这条命令效果最好,copy过来(

```shell
wmctrl -l | sed 's/^[^ ]* *[^ ]* *[^ ]* //'

wmctrl -l | sed 's/^[^ ]* *[^ ]* *[^ ]* //' |grep 'Cloud Music$'

恋ノ蟲 - 花たん — Cloud Music #成功获取标题名
```

接下来就是简单的模仿script模板了

```shell
[module/mymusic]
type=custom/script
exec=bash -c "wmctrl -l | sed 's/^[^ ]* *[^ ]* *[^ ]* //' |grep 'Cloud Music$'|sed 's/— Cloud Music//'"
format = <label>
label = %output:0:40:...%
format-foreground= #3399ff
```

效果还行,可以显示播放的曲目,但是除此之外就什么都做不了了(,可以加上点击动作,跳转到网易云音乐窗口什么的

```shell
.........
click-left=i3-msg -t command ['class=netease-cloud-music'] focus
click-right=....
```

## 从KDE connect得到的灵感

[KDE connect](https://userbase.kde.org/KDEConnect/zh-hans)是一个可以实现手机和电脑互相接受通知的一个KDE工具,只需要在应用商店安装好并和电脑连在同一个网络的时候就可以进行消息互通,文件共传等.而且手机上还有个多媒体控制,大部分情况下可以显示现在正在播放的音乐,进行一些常规多媒体操作,包括切歌,暂停等. ![Screenshot_2021-12-16-21-20-34-120_org.kde.kdeconnect_tp](/img/polybar_music/Screenshot_2021-12-16-21-20-34-120_org.kde.kdeconnect_tp.jpg)

而且不只是一两个程序,很多的软件播放音乐,甚至firefox观看视频,这里都会显示出来,说明背后应该有一套统一的协议.那我们如果了解kde connect是如何工作的,不就可以把这个模块完善的更棒吗? 

又是一番搜索,看到ubuntu论坛的这个[问题](https://askubuntu.com/questions/1046143/media-players-kde-connect-multimedia-control),题主本意是想知道如何让mpv播放的音乐被kde connect识别,第一个回答就很有帮助

![image-20211216212629748](/img/polybar_music/image-20211216212629748.png)

原来只要是走MPRIS协议的程序就可以被KDE connect识别,[MPRIS](https://specifications.freedesktop.org/mpris-spec/latest/)希望提供一套统一的API接口来管理所有的媒体播放器. [ArchlinuxWiki](https://wiki.archlinux.org/title/MPRIS)上介绍了一些与MPRIS进行通讯的工具,一眼看中了[playerctrl](https://github.com/altdesktop/playerctl),主要感觉他的操作简单直接.

```shell
~➜ playerctl metadata --format '{{artist}}-{{title}}---{{album}}'
葉月ゆら-宵闇花火---宵闇恋想奇譚

#一次只会显示一个程序,通过-p参数可以指定显示的程序
playerctl -p netease-cloud-music,mpv,%any metadata #首先选择网易云,没有再选择mpv,还是没有再选择其他
```

是不是命令非常简单直接,改进一下这是最后的版本.

```shell
[module/mymusic]
type=custom/script
exec-if=playerctl status |egrep 'Playing|Paused' #判断有没有媒体正在播放
exec=bash -c "playerctl -p netease-cloud-music,mpv,%any metadata --format '{{artist}}-{{title}}--{{album}}'"
format =  <label>
label = %output:0:40:...%
click-left=i3-msg -t command workspace 8
click-right=playerctl -p netease-cloud-music,mpv,%any play-pause#右键暂停
scroll-up=playerctl -p netease-cloud-music,mpv,%any previous#上划上一首
scroll-down=playerctl -p netease-cloud-music,mpv,%any next#下滑下一首
format-foreground= #3399ff
```

最后的效果是这样的![image-20211216215156056](/img/polybar_music/image-20211216215156056.png)


## 补充

用python脚本实现了点击跳转到播放工作区的功能,如果已经在这个工作区的时候就执行`back_and_forth`跳回到之前工作区,`i3-msg -t get_tree`返回的数据确实有点乱....

```python
#!/usr/bin/python
#author :  with <dc198424601@outlook.com>

import os
import json
import re

def get_windows():
    #获得一个字典,键是工作区名字,值是一个列表,里面是程序的classname
    classPattern=re.compile("class': '(.*?)',")#获取class名称的正则
    tree_dict={}
    p=os.popen('i3-msg -t get_tree')
    treeData=json.load(p)
    for workspace in treeData['nodes'][1]['nodes'][1]['nodes']:
        tree_dict[workspace['name']]=[]
        windows=classPattern.findall(str(workspace))
        for i in windows:
            tree_dict[workspace['name']].append(i)
    return tree_dict


def get_current_workspace():
    #返回当前工作区的名字
    p=os.popen('i3-msg -t get_workspaces')
    treeData=json.load(p)
    for i in treeData:
        if i['focused']==True:
            return i['name']


if __name__=="__main__":
    current_player=os.popen("playerctl -p netease-cloud-music,mpv,%any metadata|cut -d' ' -f1|uniq").read().replace('\n','')
    if current_player not in (get_windows()[get_current_workspace()]):
        os.system("i3-msg -t command [class={}] focus".format(current_player))
    else:
        os.system("i3-msg -t command workspace back_and_forth")

```



***

参考链接

\[1\]:[https://polybar.github.io/](https://polybar.github.io)

\[2\]:[https://www.pixiv.net/artworks/89075761](https://www.pixiv.net/artworks/89075761)

\[3\]:[https://github.com/polybar/polybar-scripts](https://github.com/polybar/polybar-scripts)

\[4\]:[https://unix.stackexchange.com/questions/342404/getting-just-the-application-name-from-wmctrl-l](https://unix.stackexchange.com/questions/342404/getting-just-the-application-name-from-wmctrl-l)

\[5\]:[https://userbase.kde.org/KDEConnect/zh-hans](https://userbase.kde.org/KDEConnect/zh-hans)

\[6\]:[https://askubuntu.com/questions/1046143/media-players-kde-connect-multimedia-control](https://askubuntu.com/questions/1046143/media-players-kde-connect-multimedia-control)

\[7\]:[https://specifications.freedesktop.org/mpris-spec/latest/](https://specifications.freedesktop.org/mpris-spec/latest/)

\[8\]:[https://wiki.archlinux.org/title/MPRIS](https://wiki.archlinux.org/title/MPRIS)

\[9\]:[https://github.com/altdesktop/playerctl](https://github.com/altdesktop/playerctl)
