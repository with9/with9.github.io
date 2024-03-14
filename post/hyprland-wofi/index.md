
## wofi介绍

[wofi](https://hg.sr.ht/~scoopta/wofi)其实就是wayland下的[rofi](https://github.com/davatorium/rofi)替代品, 对咱来说, rofi主要是一个窗口管理器和应用程序启动器, 可以轻松的切换程序和启动新的程序, 当然它还含有很多其他模块, 网上也有很多大佬自己编写的模块([文件浏览](https://github.com/marvinkreis/rofi-file-browser-extended),[网页切换](https://github.com/blackhole89/rofi-tab-switcher),[截图](https://github.com/danrog303/rofi-screenshot)等等), 最最方便的一点是可以把多个模块加在一起(-combi). 

wayland下的很多工具都追求只做好一件事情, 最经典的就是wayland下的截图了(用slurp选取位置, 返回给grim进行截图, 最后再用swappy进行后期标注), wofi相比于rofi来说轻量了许多, 自带模块只有run,drun,dmenu三者, 虽然也可以一次运行多个模块(-show drun,run,demnu), 但是没法做到像rofi这样方便的切换程序, 看了下文档, 似乎是因为wofi在wayland下面无法切换窗口, 必须通过窗管自己来实现, 并且给了一个很离谱的sway下的示例. 
```bash
WINDOW SWITCHER
       Wofi  does  not  have the ability to do window switching on its own as there is no way to do
       this with wayland/wlroots protocols however if you're using sway you can  use  swaymsg  with
       dmenu mode to accomplish it.  The following script can be used to do window switching:

       swaymsg -t get_tree |
         jq -r '.nodes[].nodes[] | if .nodes then [recurse(.nodes[])] else [] end + .floating_nodes
       | .[] | select(.nodes==[]) | ((.id | tostring) + " " + .name)' |
         wofi --show dmenu | {
           read -r id name
           swaymsg "[con_id=$id]" focus
       }
```

乍一看有点吓人,其实前面那些吓人的jq语法应该只是为了获取窗口的id和name, 然后返回给wofi的动态菜单(demnu), 用户选择好以后执行对应的命令切换窗口. 

## hyprland下wofi 窗口切换的实现

[hyprland](https://github.com/hyprwm/Hyprland)是一个wayland下的平铺式窗口管理器, 目前使用体验很棒, 文档齐全, 开发者对新功能的issue也很上心(不像隔壁sway开发者, 一个禁止缩放[xwayland的issue](https://github.com/swaywm/sway/issues/2966)开了五年多了, 更不要说什么[圆角模糊](https://github.com/swaywm/sway/issues/5998)了). hyprland下可以`hyprctl clients`获取所有窗口信息, 加上`-j`还可以直接返回json格式, 比i3和sway的tree要简单不少. 

```json
[{
    "address": "0x5aa3e870d440",
    "mapped": false,
    "hidden": false,
    "at": [0, 0],
    "size": [0, 0],
    "workspace": {
        "id": -1,
        "name": ""
    },
    "floating": false,
    "monitor": -1,
    "class": "",
    "title": "",
    "initialClass": "",
    "initialTitle": "",
    "pid": -1,
    "xwayland": true,
    "pinned": false,
    "fullscreen": false,
    "fullscreenMode": 0,
    "fakeFullscreen": false,
    "grouped": [],
    "swallowing": "0x0",
    "focusHistoryID": -1
},{
    "address": "0x5aa3e6ceb9a0",
    "mapped": true,
    "hidden": false,
    "at": [463, 169],
    "size": [992, 860],
    "workspace": {
        "id": -99,
        "name": "special"
    },
    "floating": true,
    "monitor": 0,
    "class": "com.obsproject.Studio",
    "title": "OBS 30.0.2-4 - 配置文件: 未命名 - 场景: 未命名",
    "initialClass": "com.obsproject.Studio",
    "initialTitle": "OBS 30.0.2-4 - 配置文件: 未命名 - 场景: 未命名",
    "pid": 1639111,
    "xwayland": false,
    "pinned": false,
    "fullscreen": false,
    "fullscreenMode": 0,
    "fakeFullscreen": false,
    "grouped": [],
    "swallowing": "0x0",
    "focusHistoryID": 4
},{
    "address": "0x5aa3e86fcce0",
    "mapped": false,
    "hidden": false,
    "at": [198, 163],
    "size": [1522, 913],
    "workspace": {
        "id": 2,
        "name": "2"
    },
    "floating": false,
    "monitor": 0,
    "class": "",
    "title": "",
    "initialClass": "Mailspring",
    "initialTitle": "Mailspring",
    "pid": -1,
    "xwayland": true,
    "pinned": false,
    "fullscreen": false,
    "fullscreenMode": 0,
    "fakeFullscreen": false,
    "grouped": [],
    "swallowing": "0x0",
    "focusHistoryID": -1
}]
```

下面就是模仿sway的例子实现一个hyprland版本, 因为返回内容简洁不少, 写起来其实很简单. 

```bash
hyprctl dispatch focuswindow address:$(hyprctl clients -j |jq -r '.[] | select(.title != "") | "\(.address) \(.title)"' |wofi --show dmenu|cut -f1 -d\  )
#看起来有点复杂, 其实很简单, 就是套娃多了点, 下面一层层剥开来看
#首先是获取地址和标题 
hyprctl clients -j |jq -r '.[] | select(.title != "") | "\(.address) \(.title)"'
	0x5aa3e6ceb9a0 OBS 30.0.2-4 - 配置文件: 未命名 - 场景: 未命名
	0x5aa3e6cc6120 🦊重置密码 | IBMid — Mozilla Firefox
# 然后用dmenu选择并获取地址
wofi --show dmenu|cut -f1 -d\ 
	0x5aa3e6ceb9a0
# 最后是切换过去
hyprctl dispatch focuswindow address:0x5aa3e6ceb9a0
```

但是这样我们只是实现了窗口切换, 还不能像rofi下那样又可以切换窗口又可以打开应用, 于是下面是改进版, 其实就是加上了drun.

```bash
# !/usr/bin/fish
set win_addr NULL
set win_addr (hyprctl clients -j |jq -r '.[] | select(.title != "") | "\(.address) \(.title)"' |wofi --show dmenu,drun|cut -f1 -d\  )
if test -z $win_addr
	hyprctl dispatch focuswindow address:$win_addr
end
```

![](/img/hyprland-wofi/01.png)

一开始用着蛮顺利的, 直到有一天咱想显摆一下, 遂用obs录了个屏, 运行很顺利, 但是一退出就会崩图形界面, 可以稳定复现, 除了obs外, DDNet下也存在同样问题, 调查发现是上面脚本写的有大问题, `win_addr`在启动某些程序下会变成它所有的输出内容而不是空值, 于是`focuswindow address`就会导致hyprland直接崩溃, 幸运的是调试发现传不存在的地址或者不合规的字符串并不会引起崩溃, 只有传好几行的数据时才会, 那就好办了, 不然还要写地址匹配的正则, 想想就头疼. 于是这是最后的改进版, 美化抄了一个[主题](https://github.com/7KIR7/dots/blob/main/wofi/style.css), 顺便也完善了下输出的信息.

```bash
# !/usr/bin/fish
set win_addr NULL
set win_addr (hyprctl clients -j|jq -r '.[] | select(.title != "") | "\(.address)[\(.workspace.id)]  \(.title)@\(.class)"'|wofi --show dmenu,drun -i -M fuzzy |cut -f1 -d\[|head -1)
#-i 不考虑大小写; -M fuzzy 模糊搜索

if echo $win_addr | grep ^0x 1>/dev/null
	hyprctl dispatch focuswindow address:"$win_addr"
end
```

附带最后版本的展示动图

![](/img/hyprland-wofi/02.gif)
