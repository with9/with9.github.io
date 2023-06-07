[^1]{{% music id="527479" auto="0" %}}

Zotero是一个不错的文献管理器,类似的产品有Endnotes和Mendeley,这二者功能都很强大,但是平台有一定的限制,Endnotes很难在linux平台运行,并且要进行一定的收费,Mendeley可以在linux平台运行,也不用收取费用,但是它的云同步功能在国内来说有点难以使用,同步速度很慢.Zotero相对而言,免费,可以配合坚果云搭建自己的WebDev,多平台同步(Windows,Linux,MacOS,Android..),最强大的是它的插件功能,可以轻松的在浏览网站的时候直接把想要参考的论文导入个人的文献库,同时把论文的Pdf格式作为附件下载下来,除此以外,~~把Zotero作为一个可以多平台同步的浏览器书签管理器也是不错的选择~~.
## zotero的安装

### 软件安装


去往[zotero 官网](https://www.zotero.org) 进行下载, 并进行相应的安装.

### 插件安装

推荐使用Firefox 浏览器, 用firefox 浏览器打开之前的网站, 点击Download按钮, 跳转到下载页面, 点击Install Firefox Connector, 选在添加安装.

## zotero的使用

打开zotero程序, 浏览器打开论文的地址, 点击zotero插件按钮, 就可以分析和下载论文了. 我们可以在知网的检索页点击插件按钮,进行索引文件的保存,也在论文的详情页点击插件按钮.目前位置直接在检索页点击插件还无法选中的文献进行下载.

<table><tr>
<td><img src=/img/zotero/1.png ><center>检索页</center></td>
<td><img src=/img/zotero/2.png ><center>详情页</center></td>
</tr></table>

对于一些外文的文献还支持通过索引找到原始pdf文件,不过知网的中文文献我没有成功过,使用过程可以见下图.大部分的外文文献基本都可以成功.

<table><tr>
<td><img src=/img/zotero/5.png ><center>右键获得可用pdf文件</center></td>
<td><img src=/img/zotero/6.png ><center>论文页</center></td>
</tr></table>

### cnki.js文件更新

原生的版本对于中国知网的支持不是特别好,可以从github上的一个名叫[translocators_CN](https://github.com/l0o0/translators_CN)的项目下载cnki.js并替换本地版本,可以通过打开zotero主界面的编辑-> 高级-> 文件和文件夹寻找到本地的cnki.js文件. 这个替代文件可以实现caj 文件的pdf 转换并进行下载的功能.

![ ](/img/zotero/7.png "数据库")

### word引用

一般来说安装的过程中已经自动下载并安装好了word的插件,如果没有安装好的话,可以考虑从编辑->首选项->引用->文字处理软件中进行下载和安装.插件安装好后,打开word文档,在需要插入引用的地方点击add citation,第一次会跳出样式选择按钮,这里的引用格式都不太合适,我们点击管理样式,获取更多样式,里面搜索national natural science foundation of china,或者7714,但是后者英语论文使用过程中会存在作者名称完全大写的情况. 

<table><tr>
<td><img src=/img/zotero/11.png ></td>
<td><img src=/img/zotero/12.png ></td>
</tr></table>

接着在要引用地方点击add citation 按钮, 在需要呈现参考文献的地方点击add bibliography 按钮, 呈现论文, 顺便可以在样式浏览里面浏览不同的参考文献规则样式.

## 一些小改进

打开zotero 程序, 我们可以看到知网下载的中文论文中存在将作者的姓和名分开的情况, 如果后续打算导入mendeley 或者通过姓名重命名文件的话, 非常的不美观,为了解决这一问题,我们可以找到cnki.js 文件, 找到如下内容, 添加注释并增加一行, 达到右图的标准, 应该就可以解决这个问题了.

<table><tr>
<td><img src=/img/zotero/15.png ></td>
<td><img src=/img/zotero/16.png ></td>
</tr></table>

## 配合坚果云实现多平台云同步

国内网络推荐用坚果云的第三方应用管理实现多平台同步.
### Windows以及Linux平台的设置

首先注册并登录坚果云账号,在主界面打开账号信息,切换到安全选项页面,在最下面添加一个叫做zotero的应用. 然后打开Zotero应用程序->编辑->首选项->同步,勾选文件同步并选择使用WebDAV,在下面的输入框中添加信息

```
URL:https://dav.jianguoyun.com/dav/zotero(zotero不需要额外填写)
用户名:坚果云注册的邮箱
密码:刚刚添加的zotero应用时自动生成的密码,而不是坚果云的密码

```

填写完毕后点击验证服务器,不出意外的外就设置成功了.~~出意外的化或用关键词互联网可以找到很多的解决方案~~

### Android以及IpadOS的配置

安卓应用的话推荐[Zoo for Zotero](https://github.com/mickstar/Zoo-For-Zotero),安装后右上角WdbDAV Setup填写和桌面平台一样的信息(注意这里URL填写中需要手动写包括zotero的完整路径).

Ipad可以使用papership应用,从应用商店搜索并安装papership,打开后选择用zotero登录,登录账号. 然后在设置-Zotero File Hosting中切换到WebDAV, 填写和桌面平台一样的信息(仅填写https://dav.jianguoyun.com/dav/,不用加上zotero,~~加上了也会提示你删除~~)。点击验证服务器,然后......不出意外的话你应该会得到一个错误,401或者404, 解决方法可以参考[这篇博客](https://www.jianshu.com/p/880ab833a0ba),打开坚果云网站,在zotero文件夹下新建一个空白的文本文件lastsync.txt,等一段时间后再在papership里面验证就可以了.

[^1]:顺便安利一下天国的minori社作品Eden的插曲