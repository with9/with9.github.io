[^1]{{% music id="26111634" auto="0" %}}

之前介绍了一个文献管理软件(Zotero)，我们可以自由自在的在word中引用和写作。但是我们没有办法很好的在纯文本写作的情况下随写随引。作为一个markdown重度爱好者来说，什么都要在word上写是不可忍受的。

## 简单的引用

markdown写作中，我们可以在需要引用的地方加上`[^1]`，然后在文章末尾添加`[^1]: 这是一个尾注`，在zotero里面配置好引用样式，然后利用快捷键`Ctrl+Shift+C`拷贝到剪贴板，然后粘贴到尾注部分就可以实现一条简单的引用了。

<!-- ![](/img/.zotero_and_latex/Screenshot_20210714_010708.png)

![](/img/.zotero_and_latex/image-20210714011314627.png) -->

<table><tr>
<td><img src=/img/.zotero_and_latex/Screenshot_20210714_010708.png ></td>
<td><img src=/img/.zotero_and_latex/image-20210714011314627.png ></td>
</tr></table>

## bibtex文献管理库

随便写一些小随笔的时候这样进行文献引用还行，但是一旦涉及到较多文献，或者我们的文章需要反复修改的时候，这样的简单引用就会带来一些问题，比如无法自动编号，没有办法很方便的修改引用文献样式等等，这个时候就是pandoc的用武之地了。pandoc可以实现各种文档格式之间的转换，比如markdown转word，markdown转pdf等等，pandoc还可以支持bibtex文献库的引用。bibtex是latex写作中的文献管理库的文件，这是bibtex文件中的某一条文献的内容。

```latex
@article{2018zhongguo2xingtangniaobingfangzhizhinan,
  title = {中国2型糖尿病防治指南(2017年版)},
  date = {2018},
  journaltitle = {中国实用内科杂志},
  volume = {38},
  pages = {292--344},
  issn = {1005-2194},
  abstract = {\&lt;正\&gt;1前言40年来,随着我国人口老龄化与生活方式的
变化,糖尿病从少见病变成一个流行病,糖尿病患病率从1980年的0.67\%飙升至2013年的10.4\%。相应地,科学技术的发展也带来我们对糖尿病的认识和诊疗上
的进步,血糖监测方面从只能在医院检测血糖,发展到持续葡萄糖监测、甚至无
创血糖监测,治疗方面从只有磺脲类、双胍类和人胰岛素等种类很少的降糖药,
到目前拥有二肽基肽酶IV(DPP-4)抑},
  file = {/home/with9/Zotero/storage/TZ5DP3Q4/2018_中国2型糖尿病防治
指南(2017年版).pdf},
  keywords = {2型糖尿病,guideline,type 2 diabetes,指南},
  langid = {中文;},
  number = {04}
}
```

首先是`@article{key}`说明了这是一篇论文，花括号里面是这条文献的citation-key。接下来是一些详细信息，比如标题，作者，摘要，发表年份等等。我们在需要引用的地方添加`[@2018zhongguo2xingtangniaobingfangzhizhinan]`就行了，接着利用pandoc将markdown转换为word就行了。下面是一个具体的pandoc命令。

```shell
pandoc --cite --csl=$HOME/bib/nature.csl --bibliography=$HOME/bib/my.bib test4.md -o test4.docx
#--cite --csl=后面接引用格式文件。
#--bibliography后面接我们的bibtex文献管理器
#然后是mardkdown文件test4.md -o 接输出文件名称
```
大概效果是这样。

![](/img/.zotero_and_latex/image-20210714013748389.png)

## better-bibtex for zotero

是不是感觉更加麻烦了，bibtex文件要怎么获得，cls文件又是啥。这里就要zoteo的登场啦，首先我们要安装一个插件[BETTER BIBTEX FOR ZOTERO](https://retorque.re/zotero-better-bibtex/)，登录官网，下载zotero-better-bibtex-n.n.n.xpi文件到本地，然后打开zotero，点击工具，插件，从文件安装插件，选择下载好的xpi文件就好了。重启zotero后，可以看到每个文献都自动分配了一个citation-key。

![](/img/.zotero_and_latex/image-20210714015052778.png)

打开首选项的better bibtex，可以对此插件进行一定的配置，推荐将key的格式改成和谷歌学术的一致。 `[auth:lower][year][veryshorttitle:lower]`，接下来选择文献库，导出格式选择better-bibtex，勾选keep update，这就是我们的bib文献管理库文件啦。至于csl文件，可以网上找，也可以直接用zotero的，路径在zotero文件夹的style下面。然后对一开始的pandoc命令起个别名就可以开心的写作啦。

```shell
alias pandocn 'pandoc --cite --csl=$HOME/bib/nature.csl --bibliography=$HOME/bib/my.bib'
#之后就可以这样调用啦
pandocn A.md -o A.docx
```



<!-- ![](/img/.zotero_and_latex/image-20210714015412520.png)

![](/img/.zotero_and_latex/image-20210714015848499.png) -->

<table><tr>
<td><img src=/img/.zotero_and_latex/image-20210714015412520.png ></td>
<td><img src=/img/.zotero_and_latex/image-20210714015848499.png ></td>
</tr></table>

我们现在写作是比之前方便了一丢丢，但是citation-key要怎么弄呢，难道我们要手动复制吗？当然不用。官网上有一个vim引用的脚本。我们简单的查看一下。

```shell
function! ZoteroCite()
  " pick a format based on the filetype (customize at will)
  let format = &filetype =~ '.*tex' ? 'citep' : 'pandoc'
  let api_call = 'http://127.0.0.1:23119/better-bibtex/cayw?format='.format.'&brackets=1'
  let ref = system('curl -s '.shellescape(api_call))
  return ref
endfunction

noremap <leader>z "=ZoteroCite()<CR>p
inoremap <C-z> <C-r>=ZoteroCite()<CR>
```

可以看到实际上执行的命令就是`curl -s 某个网站`，我们直接在shell中运行一下，跳出一个框框，就和我们在word上面写作没有什么区别。选择好引用的文献后，点击ok。可以看到输出了一个文献的key，那么利用xdotool 和xclip就可以写一个随处引用的小脚本啦。

<!-- ![](/img/.zotero_and_latex/image-20210714021119575.png)

![](/img/.zotero_and_latex/image-20210714021312563.png) -->

<table><tr>
<td><img src=/img/.zotero_and_latex/image-20210714021119575.png ></td>
<td><img src=/img/.zotero_and_latex/image-20210714021312563.png ></td>
</tr></table>

```bash
#!/bin/bash -e
chars=$(curl -s 'http://127.0.0.1:23119/better-bibtex/cayw?format=pandoc&brackets=1')
echo -n $chars | xclip -selection clipboard #获取key并存在剪切板里面
sleep 0.4
# xdotool type  "$chars" #一开始试着直接打出来，如果目前是中文输入法的话就会出现一些糟糕的情况，比如「@溜018基于包大豆inzucexuk]，所以选择用粘贴版来处理

xdotool key ctrl+v #进行粘贴
copyq remove #我的剪切板管理器，可以无视
```

## 一些补充

接着介绍一下typora的妙用，Typora是一个很不错的markdown文本编辑器，最近发现它的导出可以自定义命令，利用它我们可以实现一些简单的功能，比如自定义导出的pandoc命令。

```shell
pandoc --cite --csl=$HOME/bib/nature.csl --bibliography=$HOME/bib/my.bib ${currentFileFullName} -o ${currentFileName}.docx #定义导出命令，导出到本地同名的docx文件
bash $HOME/my_shell/typorashell/diary #是复制模板md文件生成每天的日记模板，下面是diary脚本
#!/bin/bash
cur_date="`date +%Y-%m-%d`"
filename="$HOME/Documents/Obsidian/Notes/Diary/$cur_date.md"
dirayname="$HOME/Documents/Obsidian/Notes/模板/DiaryEx.md"
if [[ ! -e $filename ]];then
    cp $dirayname $filename
fi
```

最后是修改导出的docx模板，pandoc的文档中提到，pandoc导出成docx文件的时候默认会选择`$HOME/.pandoc/reference.docx `文件作为模板，也就是说这里面定义的样式是什么，生成的文件就按什么样式，我们可以在这个文件里面定义好我们的样式，当然pandoc也支持`--reference-docx reference.docx `参数来临时选择模板。

```shell
pandoc --print-default-data-file reference.docx > reference.docx #在默认路径下没有看到 reference.docx时候可以通过这个命令来生成。
pandoc --reference-docx reference.docx A.md -o A.docx#临时调用模板
```

## 太长不看

最后总结一下流程（致太长不看）——此脚本只适用于linux，win应该有类似的，没有研究过

1. 下载安装better-bibtex插件，简单配置后导出文献库。

1. 找到style文件`Zotero/styles/*.csl`

1. 编写xclip，xdotool脚本，也可以直接复制我写的。并绑定桌面快捷键。

    ```shell
    #!/bin/bash -e
    chars=$(curl -s 'http://127.0.0.1:23119/better-bibtex/cayw?format=pandoc&brackets=1')
    echo -n $chars | xclip -selection clipboard 
    sleep 0.4
    xdotool key ctrl+v 
    copyq remove 
    ```

1. 在需要引用的地方按快捷键，选择引用文献

1. 用pandoc命令将markdown文件转换为docx文件`pandoc --cite --csl=你的csl文件路径.csl --bibliography=你的文献库路径.bib A.md -o A.docx`(推荐定义一个别名，typora可以自定义导出命令)

[^1]: 蝉在叫，人坏掉(