翻译自[opensource](https://opensource.com/article/19/8/how-linux-desktop-grown)的一篇文章，作者是: Jim Hall ，非常有趣，介绍了Linux桌面的发展历史。

> Since the early 1990s, the Linux desktop has matured from a simple window manager to a full desktop. Join us on a journey through the history of the Linux desktop.

从20世纪90年代以来，Linux桌面已经从简单的窗口管理器成熟为完整的桌面了，让我们一起来回顾一下Linux桌面的历史吧。

![ ](https://opensource.com/sites/default/files/styles/image-full-size/public/lead-images/1980s-computer-yearbook.png?itok=eGOYEKK- "LSE Library. Modified by Opensource.com. CC BY-SA 4.0")

***Image by :*** *LSE Library. Modified by Opensource.com. CC BY-SA 4.0*

>I first installed Linux in 1993. At that time, you really didn't have many options for installing the operating system. In those early days, many people simply copied a running image from someone else. Then someone had the neat idea to create a "distribution" of Linux that let you customize what software you wanted to install. That was the Softlanding Linux System (SLS) and my first introduction to Linux.

我第一次安装Linux是在1993年。那时候，对于安装系统，你确实没有太多选择。大家安装Linux系统都是简单的拷贝其他人正在运行的磁盘映像(*Image*)。后来某人有了个非常秒的想法：创造一个Linux发行版(*Linux Distributions*)使得我们可以自定义想要安装的软件。那就是**软着落Linunx 系统**(*Softlanding Linux System,SLS*)，这也是我第一次接触到的Linux。

>My '386 PC didn't have much memory, but it was enough. SLS 1.03 required 2MB of memory to run, or 4MB if you wanted to compile programs. If you wanted to run the X Window System, you needed a whopping 8MB of memory. And my PC had just enough memory to run X.

我的'386 电脑内存不是很多，但是足够安装SLS， SLS1.03的运行内存要求是2MB，但是如果你想要编译程序的话，那么你需要4MB内存。如果是要运行X 窗口系统(*X Window System*)，那么你需要8MB的内存。我的电脑配置刚好可以运行X 窗口系统。

>As I'd grown up with the command line, a graphical user interface wasn't essential to me. But it sure was convenient. I could run applications in different windows and easily switch between tasks.

由于我是从命令行(*command line*)过来的，所以图形用户界面(*graphical user interface*)对我来说并不是必不可少的。但是图形界面确实非常方便。我可以在不同的窗口运行程序，也可以轻松的切换任务。

>From my first experiment with Linux, I was hooked. I've stuck with Linux on my desktop ever since. Like many people, I ran Linux in a dual-boot configuration for a while so I could jump back to MS-DOS and Windows to run certain programs. Until 1998, when I finally took the plunge and went all-in with Linux.

第一次使用Linux的时候，我就被深深吸引了。自此以后，Linux就一直在我的电脑中。和许多人一样，我通过双引导配置(*dual-boot configuration*)来运行Linux，这样的话，我可以方便切回MS-DOS 和Windows 系统来运行某些特定程序。直到1998年我才完全放弃其他系统。

> Over the last 26 years, I have watched the Linux desktop mature. I've also tried an interesting combination of desktop environments over that time, which I'll share by taking a journey through the history of the Linux desktop.

在过去的26年，我见证了Linux桌面的成长。在那段时间里面，我也尝试了各种桌面环境，下面我会在介绍Linux桌面历史的同时分享我的经历。

# X和桌面管理器

>The first "desktops" on Linux weren't yet desktops. Instead, they were *window managers* running on the X Window System. X provided the basic building blocks for a graphical user interface, such as creating windows on the screen and providing keyboard and mouse input. By itself, X didn't do much. To make the X graphical environment useful, you needed a way to manage all the windows in your session. That's where the *window manager* came in. Running an X program like xterm or xclock opens that program in a window. The window manager keeps track of windows and does basic housekeeping, such as letting you move windows around and minimize them. The rest is up to you. You could launch programs when X started by listing them in the **~/.xinitrc** file, but usually, you'd run new programs from an xterm.

Linux的第一个"桌面"还不是桌面。准确说，它们是运行在X 桌面系统(*X Window System*)的窗口管理器(*window managers*)。 X提供了诸如在屏幕上创建窗口这样的图形用户界面的基本构建块， 同时也提供了键盘和鼠标的输入。 X自身没有做什么。 为了使用X图形环境(*X graphical environment*)， 你需要某种方法来管理会话(session)中的所有的窗口。 那就是窗口管理器。 运行一个诸如xterm或xclock的图形程序时，它会在一个窗口打开这个程序。 窗口管理器跟踪这些窗口并提供基本的内务服务(*basic housekeeping*)， 诸如在窗口间切换以及最小化。 剩下的取决于你啦。 你可以通过把它们添加进`~/.xinitrc`文件，使得运行X的时候打开这些程序， 不过通常来说，你会从[xterm](https://invisible-island.net/xterm/)来运行新程序。

>The most common window manager in 1993 was TWM, which dates back to 1988. TWM was quite simple and provided only basic window management.

1993年最受欢迎的窗口管理器是[TWM](https://wiki.archlinux.org/index.php/Twm)，它于1988年问世。 

TWM非常简陋，只提供基本窗口管理。 

![ ](https://opensource.com/sites/default/files/uploads/twm-sls105.png "TWM on SLS 1.05")

{{% center %}}

**TWM on SLS 1.05 showing xterm, xclock, and the Emacs editor**

{{% /center %}}

>Yet another early window manager was the OpenLook Virtual Window Manager (OLVWM). OpenLook was a graphical user interface developed by Sun Microsystems in the 1980s and later ported to other Unix platforms. As a *virtual* window manager, OLVWM supported multiple workspaces.

另一个早期的桌面管理器是OpenLook桌面管理器(*OpenLook Virutal Window Manager*,OLVWM).OpenLook是[太阳计算机系统]([https://zh.wikipedia.org/wiki/%E6%98%87%E9%99%BD%E9%9B%BB%E8%85%A6](https://zh.wikipedia.org/wiki/昇陽電腦))(Sun MIcrosystems)上世纪80年代开发的图形用户界面，之后也被移植到其他Unix平台。作为一个虚拟图形管理器，OLVWM支持多个工作区。

![](https://opensource.com/sites/default/files/uploads/olvwm-sls105.png "OLVWM on SLS 1.05")

{{%  center  %}}

**OLVWM on SLS 1.05 showing xterm and the Virtual Workspaces selector**

{{% /center %}}

>When Linux began to grow in popularity, it didn't take long for others to create new window managers with smoother performance and improved interfaces. The first of these new window managers was FVWM, a virtual window manager. FVWM sported a more modern look than TWM or OLVWM. But we didn't yet have a desktop.

当Linux逐渐普及的时候，人们很快创建了具有更加流畅的性能和改进的用户界面的新的窗口管理器。第一个新的窗口管理器是FVWM，相比TWM和OLVWM，它有更加时尚的界面。但是我们还是没有得到真正的桌面。

![](https://opensource.com/sites/default/files/uploads/fvwm-sls105.png "FVWM on SLS 1.05")

{{%  center  %}}

**FVWM on SLS 1.05 showing xterm and a file manager**

{{% /center %}}

>To modern eyes, TWM and FVWM may look pretty plain. But it's important to remember what other graphical environments looked like at the time. The then-current version of Windows looked rather simple. Windows versions 1 through 3 used a plain launcher called the Program Manager.

用现在的眼光来看，TWM和FVWM可能看起来非常的简陋。但是如果我们看一下同时代的其他图形环境你就会改变看法啦。当时的Windows版本看起来更加简陋。Windows 1 到Windows 3 使用一个称为程序管理器(*Program Manage*r)的简单启动器。

![](https://opensource.com/sites/default/files/uploads/win311.png "Windows 3.11")

{{%  center  %}}

**Windows 3.11 showing the Program Manager and the Notepad editor**

{{% /center %}}

>In August 1995, Microsoft released Windows 95 and changed the modern PC desktop landscape. Certainly, I was impressed. I thought Windows 3.x was ungainly and ugly, but Windows 95 was smooth and pretty. More importantly, Windows 95 was what we now consider a desktop. The new desktop metaphor was a huge step forward. You could put icons on the desktop—and in fact, Windows 95 presented two default desktop icons, for My Computer (to open a file manager) and the Recycle Bin (where you put files to be deleted later).

1995年8月，微软发布了Windows95，它改变当时的电脑桌面布局。我认为Windows 3.X是不够优雅或者说是丑陋的，但是我认为Windows95是优雅并且美丽的。更重要的是，Windows95就是我们现在所认为的桌面应该的样子。新的[桌面比拟](https://zh.wikipedia.org/wiki/%E6%A1%8C%E9%9D%A2%E6%AF%94%E6%93%AC)(*Desktop Metaphor*)向前迈进了一大步。你可以把图标放在桌面上，Windows95预先放置了两个默认的桌面图标{{< pinyin 我的电脑 "My Computer">}}(打开{{< pinyin 文件管理器 "file manager" >}})和{{<pinyin 回收站 "Recycle bin" >}}(里面放置你稍后将删除的文件)。

>But more importantly, the Windows 95 desktop meant *integration*. The Program Manager was gone, replaced by a Taskbar at the bottom of the screen that let you launch new programs using a simpler Start menu. The Taskbar was multifunctional and also showed your running programs via a series of buttons and a dock showing the time, speaker volume, and other simple controls. You could right-click on any object on the new desktop, and Windows 95 would present you with a context-sensitive menu with actions you could perform.

更重要的是,Windows95桌面意味着集成(*integration*)。程序管理器消失了，取而代之的是桌面底下的一个任务栏(*Taskbar*)，你可以通过点击开始菜单(*Start Menu*)来打开一个新的程序。任务栏是多功能的，上面有一系列的按钮来显示你正在运行的程序，还有一个小的托盘(*Dock*)，上面有展示时间，控制音量和一些其他功能的小控件。你可以在桌面上的任何对象上右键点击鼠标，Windows95会展现出一个相关菜单，其中包含有你可以执行的操作。

![](https://opensource.com/sites/default/files/uploads/win95.png "Windows 95")

{{%  center  %}}

**Windows 95 showing the Notepad editor**

{{% /center %}}

>The Windows 95 interface was slick and much easier to use than previous versions of Windows—and even other Linux window managers. Not to be outdone, Linux developers created a new version of FVWM that mimicked the Windows 95 interface. Called FVWM95, the new window manager still wasn't a desktop, but it looked very nice. The new taskbar let you start new X programs using the Start menu. The taskbar also showed your running programs using buttons similar to Windows 95's.

Windows95的界面非常的优雅，并且比起之前的Windows版本和Linux窗口管理器更加易于使用。为了不被淘汰，Linux开发者开发了新版本的FVWM，它在界面上模仿了Windows95。 它被称作FVWM95，尽管看起来非常优美，但它仍不是真的桌面。新的任务栏使你可以通过开始菜单运行图形程序。它也采用了类似Windows95的方式，用按钮展示你正在运行的程序。

![](https://opensource.com/sites/default/files/uploads/fvwm95-rh52.png "FVWM95 on Red Hat Linux 5.2")

{{%  center  %}}

**FVWM95 on Red Hat Linux 5.2 showing xterm and a quick-access program launcher with icons for xterm, the file manager, and other programs**

{{% /center %}}

>While FVWM95 and other window managers were improving, the core problem remained: Linux didn't really have a desktop. It had a collection of window managers, and that was about it. Linux applications that used a graphical user interface (GUI, pretty much meaning they were X applications) all looked different and worked differently. You couldn't copy and paste from one application to another, except the simple text-only copy/paste provided by the X Window System. What Linux really needed was a complete redo in its GUI to create the first desktop.

尽管FVWM95和其他窗口管理器在不断的改进，但是核心问题依然没有解决——Linux没有真正的桌面。它有一组桌面管理器，仅此而已。用图形界面(*Graphical user interface*   GUI)的Linux应用看起来不同，他们的工作方式也不一样。除了X桌面系统提供的简单文字拷贝以外，你不能从一个应用程序里面拷贝内容到另一个应用程序。Linux真正需要的是GUI的完全重构，从而可以创造出第一个真正意义上的桌面。

# The first Linux desktop

>In 1996, Matthias Ettrich was troubled by the inconsistency of Linux applications under X. He wanted to make the graphical environment easy to use. And more importantly, he wanted to make everything integrated—like an actual desktop.

1996年，马蒂亚斯·埃特里希([Matthias Ettrich](https://zh.wikipedia.org/wiki/%E9%A9%AC%E8%92%82%E4%BA%9A%E6%96%AF%C2%B7%E5%9F%83%E7%89%B9%E9%87%8C%E5%B8%8C))对于图形界面下的Linux应用程序不一致很是困扰。他希望使图形环境更加易于使用。最重要的是，他希望让一切整合在一起——就像正真的桌面一样。

> Matthias started work on the K Desktop Environment. That's K for "Kool." But the name KDE was also meant to be a play on the Common Desktop Environment (CDE) that was the standard in the "Big Unix" world. Although by 1996, CDE was looking pretty dated. CDE was based on the Motif widget set, which is the same design that FVWM mimicked. Finalized in July 1998, KDE 1.0 was a definite improvement over plain window managers like FVWM95.

马蒂亚斯开始研究K桌面环境(*K DesktopEnvironment*)。"K"的意思是"Kool"。KDE这个名字也意味着它可以在广大的Unix世界的通用桌面环境(*Common Desktop Environment* CDE)中发挥作用。尽管已经到了1996年，CDE仍然看起来非常的过时。CDE基于主题部件集(*Motif widget set*)，和FVWM的设计一样。KDE1.0完成于1998年，相对于FVWM等简陋的桌面管理器来说，它是十足的进步。

![](https://opensource.com/sites/default/files/uploads/kde1.png "KDE 1.0")

{{%  center  %}}

**K Desktop Environment (KDE) version 1.0**

**Image credit: Paul Brown / KDE**

{{% /center %}}

>KDE was a big step forward for Linux. Finally, Linux had a true desktop with application integration and more modern desktop icons. KDE's design was not dissimilar from Windows 95. You had a kind-of taskbar along the bottom of the screen that provided the equivalent of Windows 95's Start menu as well as several application shortcuts. KDE also supported virtual desktops, which were cleverly labeled One, Two, Three, and Four. Running applications were represented via buttons in a separate taskbar at the top of the screen.

KDE是Linux系统向前迈进了一大步。Linux从此拥有了具有应用整合与现代化的桌面图标的正真桌面。KDE的设计与Windows 95并不不同。你可以在屏幕底部看到一个任务栏，它提供类似windows 95的开始菜单的功能，同时上面也有一些应用的快捷方式。 KDE还支持虚拟桌面，通过标签一，标签二，标签三，标签四的方式清晰的展示着。正在运行的程序会通过屏幕顶部单独的任务栏中的按钮表示出来。

>But not everyone was happy with KDE. To abstract the GUI from the system, KDE used Trolltech's Qt toolkit library. Unfortunately, Qt was not distributed under a free software license. Trolltech allowed Qt to be used at no charge in free software applications but charged a fee to use it in commercial or proprietary applications. And that dichotomy is not aligned with free software. This caused problems for Linux distributions: Should they include KDE? Or default to an older but free software graphical user interface like FVWM?

但是不是所有人都对KDE感到满足。为了从屏幕中抽提GUI，KDE使用了奇趣科技(*Trolltech*)的Qt工具包库。遗憾的是，Qt不是以免费软件授权(*free software license*)的方式进行发行的。奇趣科技允许在自由软件免费使用Qt，但是在商业使用或者私有软件中会收取费用。这种分别对待的措施与自由软件理念不符。这导致了Linux发行版的一个问题:它们应不应该默认包含KDE，还是选择依旧默认包含一个比较老旧，但是完全自由的图形用户界面(比如FVWM)呢?

>In response, Miguel de Icaza and Federico Mena started work in 1997 on a new Linux desktop. The new project was dubbed GNOME, for GNU Network Object Model Environment. GNOME aimed to be completely free software and used a different toolkit, called GTK, from the GIMP image editor. GTK literally stood for GIMP Tool Kit. When GNOME 1.0 was finally released in 1999, Linux had another modern desktop environment.

为此，米格尔·德伊卡萨( *Miguel de Icaza*)和费德里科·梅纳(*Federico Mena*)于1997年开始研究一个新的Linux桌面。这个项目就是GNOME，意思是GNU网络对象模型环境(*GNU Network Object Model*)。GNOME的目标是打造一个完全自由的软件，它使用GIMP图像编辑器中一个称作GTK的新工具组(*toolkit*)。GTK字面意思是GIMP的工具组。1999年，随着第一版的GNOME最终发行，Linux有了另一个现代的桌面。

![](https://opensource.com/sites/default/files/uploads/gnome10.png)

{{% center %}}

**GNOME version 1.0**  
**Image credit: GNOME Documentation Project** 

{{% /center %}}

>While it was great to have two desktop environments for Linux, the "KDE versus GNOME" rivalry continued for some time. By 1999, Trolltech re-released the Qt library under a new public license, the Q Public License (QPL). But the new license carried its own baggage—the QPL only applied to Qt's use in open source software projects, not commercial projects. Thus the Free Software Foundation deemed the QPL not compatible with the GNU General Public License (GNU GPL). This licensing issue would remain until Trolltech re-re-released the Qt library under the GNU GPL version 2 in 2000.

有两个Linux桌面环境固然很棒，但“ KDE与GNOME”的竞争仍持续了一段时间。 1999年奇趣科技根据新的公共许可证(Q 公共许可证(QPL))重新发布了Qt工具库。 但是新的许可证仅适用于Qt在开源项目中使用， 不适用于商业项目。 因此自由软件基金组织认为QPL与GPL不兼容。 这个问题持续到2000年，奇趣科技在这一年根据GPL发行了第二版的Qt工具库。

# Development over time

>The Linux desktop continued to mature. KDE and GNOME settled into a friendly competition that pushed both to add new features and to exchange ideas and concepts. By 2004, both GNOME and KDE had made significant strides, yet brought only incremental changes to the user interface.

Linux桌面在不断的发展。 KDE与GNOME开始了一场友谊竞赛， 这场竞赛推动了二者新特性的增加，也加强了二者间想法和概念的交流。 到2004年， GNOME和KDE都获得了巨大的进步， 不过仅仅是对用户界面的一些小修改。 

>KDE 2 and 3 continued to rely on a taskbar concept at the bottom of the screen but incorporated the buttons for running applications. One of KDE's most visible changes was the addition of the Konqueror browser, which first appeared in KDE 2.

KDE 2和3继续依赖屏幕底部的任务栏概念， 但并入了用于运行应用程序的按钮。 KDE的一大视觉变化是加入了Konqueror浏览器， 它第一次登场是在KDE 2。 

![](https://opensource.com/sites/default/files/uploads/kde_2.2.2.png)

{{% center %}}

**KDE 2.2.2 (2001) showing the Konqueror browser**  
**Image credit: Paul Brown / KDE**

{{% /center %}}

![](https://opensource.com/sites/default/files/uploads/kde322-fc2.png)

{{% center %}}

**KDE 3.2.2 (2004) on Fedora Core 2 showing the Konqueror file manager (using a Fedora Core 2 theme)** 

{{% /center  %}}

>GNOME 2 also used a taskbar concept but split the bar into two: a taskbar at the top of the screen to launch applications and respond to desktop alerts, and a taskbar at the bottom of the page to show running applications. On my own, I referred to the two taskbars as "things you can do" (top) and "things are you doing" (bottom). In addition to the streamlined user interface, GNOME also added an updated file manager called Nautilus, developed by Eazel.

GNOME 2也使用了任务栏的概念，但是它将任务栏分成了两部分: 一个任务栏在屏幕顶端，显示桌面提醒; 另一个在页面的底部，显示正在运行的应用。 对我来说，我认为这两个任务栏分别是"你可以做的事"(顶部)和"你正在做的事"(底部)。 除了简化的用户界面外， GNOME还加入了最新的文件管理器——Nautilus， 该管理器由Eazel公司开发。 

![](https://opensource.com/sites/default/files/uploads/gnome26-fc2.png)

{{% center %}}

**GNOME 2.6.0 (2004) on Fedora Core 2 showing the Nautilus file manager (using a Fedora Core 2 theme)**

{{% /center %}}

>Over time, KDE and GNOME have taken different paths. Both provide a feature-rich, robust, and modern desktop environment—but with different user interface goals. In 2011, there was a major deviation between how GNOME and KDE approached the desktop interface. KDE 4.6 (January 2011) and KDE 4.7 (July 2011) provided a more traditional desktop metaphor while continuing to rely on the taskbar concept familiar to many users. Of course, KDE saw lots of changes under the hood, but the familiar look and feel remained.

随着时间的推移，KDE和GNOME采取了不同的方法，以不同的用户界面目标同时开发了界面丰富，强大现代的桌面环境。在2011年，GNOME和KDE处理桌面界面的方式之间存在重大差异。KDE4.6（2011.1）和KDE（2011.7）提供了一个更加传统的[桌面比拟](https://zh.wikipedia.org/wiki/%E6%A1%8C%E9%9D%A2%E6%AF%94%E6%93%AC)，同时继续保留许多用户熟悉的任务管理器概念。当然，KDE虽然发生了很多引擎上的变化，但是仍然保留用户熟悉的外观。

![](https://opensource.com/sites/default/files/uploads/kde46.png)

{{% center %}}

KDE 4.6 showing the Gwenview image viewer   
Image credit: KDE

{{% /center %}}

>In 2011, GNOME completely changed gears with a new desktop concept. GNOME 3 aimed to create a simpler, more streamlined desktop experience, allowing users to focus on what they were working on. The taskbar disappeared, replaced by a black status bar at the top of the screen that included volume and network controls, displayed the time and battery status, and allowed users to launch new programs via a redesigned menu.

在2011年，GNOME则完全改变了它的桌面概念。GNOME 3旨在创建更简单，更简化的桌面体验，它允许用户集中于正在进行的活动。任务管理器被一个在屏幕顶部的状态栏替代，它包括有音量和网络控制，同时展示时间和电池状态，并且允许用户通过重新设计的用户菜单打开新的程序。

>The menu was the most dramatic change. Clicking the Activities menu or moving the mouse into the Activities "hot corner" showed all open applications as separate windows. Users could also click an Applications tab from the Overview to start a new program. The Overview also provided an integrated search function.

菜单是最戏剧性的变化。点击活动菜单或者把鼠标移动到“热区”，可以展示所用打开的应用，他们会以单独窗口的形式展示出来。用户也可以通过点击总览界面的应用标签来打开一个新的程序。总览界面也提供整合搜索的功能。

![](https://opensource.com/sites/default/files/uploads/gnome30.png)

{{% center %}}

GNOME 3.0 showing the GNOME Pictures application  
Image credit: GNOME

{{% /center %}}

![](https://opensource.com/sites/default/files/uploads/gnome30-overview.png)

{{% center %}}

GNOME 3.0 showing the Activities Overview  
Image credit: GNOME

{{% /center %}}



# Your Choice of desktop

>Having two desktops for Linux means users have great choice. Some prefer KDE and others like GNOME. That's fine. Pick the desktop that best suits you.

拥有两个Linux桌面意味着用户有很多选择。一些人喜欢KDE，一些人更喜欢GNOME，这都没有问题。选择适合你的就行。

>To be sure, both KDE and GNOME have fans and detractors. For example, GNOME received a fair bit of criticism for dropping the taskbar in favor of the Activities Overview. Perhaps the most well-known critic was Linus Torvalds, who [loudly denounced and abandoned](https://www.theregister.co.uk/2011/08/05/linus_slams_gnome_three/) the new GNOME as an "unholy mess" in 2011—before [moving back](https://www.phoronix.com/scan.php?page=news_item&px=MTMxNjc) to GNOME two years later.

当然，二者既有拥护者也有反对者。举个例子，GNOME因为用活动概述替代任务栏而受到了很多的批评。最出名的批评来自林纳斯·托瓦兹，他在2011年大声谴责并放弃新的GNOME，将其视为“邪恶的混乱”，两年后他回归了GNOME。

>Others have made similar criticisms of GNOME 3, to the point that some developers forked the GNOME 2 source code to create the MATE desktop. MATE (which stands for MATE Advanced Traditional Environment) continues the traditional taskbar interface from GNOME 2.

其他人对GNOME3也作出了相似的批评，以至于有些开发人员分叉GNOME 2源代码来创建MATE桌面。MATE桌面从GNOME2延续了传统的任务栏界面。

>Regardless, there's no doubt that the two most popular Linux desktops today are KDE and GNOME. Their current versions are both very mature and packed with features. Both KDE 5.16 (2019) and GNOME 3.32 (2019) try to simplify and streamline the Linux desktop experience—but in different ways. GNOME 3.32 continues to aim for a minimal appearance, removing all distracting user interface elements so users can focus on their applications and work. KDE 5.16 takes a more familiar approach with the taskbar but has added other visual improvements and flair, especially around improved widget handling and icons.

不管如何，毫无疑问当今最受欢迎的Linux桌面环境是KDE和GNOME。他们的当今版本都很成熟，并且充满特色。KDE 5.16和GNOME 3.32都从不同角度尝试简化Linux桌面体验。GNOME3.32继续追求外观最小化，通过删除所有会使用户分心的元素来确保用户可以专注于应用和工作。KDE 5.16采用用户更加熟悉的任务栏，但是增加了一些视觉上的改动，尤其是改进了小部件和图标。

![](https://opensource.com/sites/default/files/uploads/kde516.png)

{{%center%}}

KDE 5.16 Plasma  
Image credit: KDE

{{%/center%}}

![](https://opensource.com/sites/default/files/uploads/gnome332.png)

{{%center%}}

GNOME 3.32  
Image credit: GNOME

{{%/center%}}

>At the same time, you don't completely lose out on compatibility. Every major Linux distribution provides compatibility libraries, so you can run applications from, say, KDE while running GNOME. This is immensely useful when an application you really want to use is written for the other desktop environment—not a problem; you can run KDE applications on GNOME and vice versa.

同一时间，你不需要牺牲兼容性。每个主流Linux发行版都提供兼容性库，因此你可以运行GNOME的同时运行KDE。当你想要运行的程序是为其他桌面环境所写的时候，这很有用。您可以在GNOME上运行KDE应用程序，反之亦然。

>I don't see this changing anytime soon. And I think that's a good thing. Healthy competition between KDE and GNOME has allowed developers in both camps to push the envelope. Whether you use KDE or GNOME, you have a modern desktop with great integration. And above all, this means Linux has the best feature in free software: choice.

我认为这种情况不会很快改变。同时我认为这是一件好事。KDE和GNOME的良性竞争使这两个阵营的开发人员都可以挑战极限。无论您使用KDE还是GNOME，您都将拥有一个具有高度集成性的现代化桌面。最重要的是，这意味着Linux在自由软件中具有的最佳功能：选择。