---
layout: article
title: 我的LaTeX小结（二）：工具和排版引擎
tags:
  - Uncategorized
  - LaTeX
date: 2015-08-07 00:07:51
keywords: LaTeX, CTeX, Tex Live, pdfLaTeX, XeLaTeX
intro: 几年下来对LaTeX的使用积累了一些经验，对于编辑器和编译引擎有了更深入的认识，写一点自己的想法和建议。
---

[我的LaTeX小结（一）：LaTeX基础](https://moongazer.me/2013/08/30/latex-notes-i/ "我的LaTeX小结（一）：LaTeX基础")

本来没想到会写（二），不过最近一年多用LaTeX有了一些新的想法，因此记录在这里。

<!-- more -->

## 工具

我这里说的工具包括TeX软件套装和编辑器，二者又是关联的。我先说一下我个人的建议：**如果不是急着用或者有其它情况，不要使用CTeX套装，请自己安装Tex Live或者MiKTeX，配合自己喜欢的编辑器使用**。下面具体介绍。<span id="more-121"></span>

### 软件套装

简单说一下不建议使用CTeX套装的原因：

1.  已经很久不更新了
2.  在安装和卸载时候有一些bug，如环境变量的问题
3.  套装中的WinEdt为破解版
4.  对于编码的支持不好（实际上是WinEdt太愚蠢了）

3属于原则性的问题，不过重不重要取决于每个人自己了（实际上我搞不懂WinEdt这么烂的编辑器怎么好意思收费还卖这么贵？）。

关于Tex Live和MikTex哪个好，网上有很多相关的内容，我并不关心，只能说我自己用的是Tex Live而且只用过这个，感觉还行。

对于Windows系统，安装程序在[这里](https://www.tug.org/texlive/acquire-netinstall.html "Tex Live 安装")下载，下载“install-tl-windows.exe”即可。这个是在线安装的，要比CTeX那样下载完安装包再安装要慢一些，不过由于CTAN的镜像很多，这个安装时间应该也能接受。在国内的话可能要三四个小时才能全部搞完，不过多数时间都是自动完成的，电脑放着就行了。

下载完后（建议以管理员身份）运行安装程序，选择Custom Install，稍后会跳出来一个新的界面，建议在安装方案中选择第二项“medium scheme (small + more packages and languages)”，在下面Install collections中参考我的选择：

![](http://ww4.sinaimg.cn/large/413ed6d6gw1eutupy25x5j20k30e2acn.jpg)

确定之后自己选择安装路径，其它使用默认配置，安装，然后慢慢等就行了。

关于Linux下Tex Live的安装和使用，Archlinux的[wiki](https://wiki.archlinux.org/index.php/TeXLive_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))写得很清楚，其它linux发行版可以参照着进行。

安装完之后，如果在使用的过程中发现编译时缺少某个包（往往是在之前安装的时候漏掉了），可以通过以下命令安装和卸载：

```
tlmgr install
tlmgr remove
```

在后面接上包的名字。包的名字不一定和.sty文件的文件名相同，怎么找就要靠自己的智慧了，一般搜索引擎（Google）上搜一下“***.sty not found”能解决。

常用的包包括：ctex、CJK、cjkpunct、zhmetrics、subfigure，它们的功能通过名字应该就可以看出来。

使用TeX Live的话一般都使用UTF8编码，后面还会提到。

### 编辑器

我自己常用的编辑器包括Notepad++和Vim，有能力购买并使用Sublime Text的当然也没问题。下面简单介绍这几个编辑器如何配合Tex Live套装使用。

第一种方法是相对蛋疼的一种，即平时使用编辑器编辑，需要编译的时候打开TexWorks进行编译，没有问题则返回到原来的编辑器继续，这么搞也比较简单。

第二种方法是使用插件或脚本等方法，直接在编辑器内编译，完成之后调用pdf查看器查看。这里建议使用[Sumatra PDF](http://www.sumatrapdfreader.org/free-pdf-reader.html)，可以使用反向搜索。

对于**Notepad++**，参见

http://bbs.ctex.org/forum.php?mod=viewthread&#038;tid=49230

需要在Notepad++上安装NppExec插件，然后将特定的一段代码保存为常用代码，通过快捷键调用运行。

Sumatra PDF的反向搜索命令行为

```
&quot;D:\Program Files (x86)\Notepad++\notepad++.exe&quot; -n%l &quot;%f&quot;
```



对于**Sublime Text**，参见

http://www.cnblogs.com/dezheng/p/3895249.html

需要借助LaTeXTools插件。

Sumatra PDF的反向搜索命令行为

```
&quot;C:\Program Files\Sublime Text 3\sublime_text.exe&quot; &quot;%f&quot;:%l&quot;
```

对于**Vim**，参见

http://blog.sina.com.cn/s/blog_5e16f1770100fqyt.html

Sumatra PDF的反向搜索命令行为

```
&quot;D:\Program Files (x86)\Vim\vim74\gvim.exe&quot; -c &quot;:RemoteOpen +%l %f&quot;
```

P.S. Sumatra PDF的反向搜索功能的设置很诡异。刚安装好的时候“设置”里面是没有的，需要至少打开一次latex编译好的文档之后才会出现。

## 排版引擎

在写LaTeX小结（一）的时候很naive，完全不了解排版引擎的问题，只是简单地引用了ctex包然后直接用CTeX套装里的WinEdt编译完事。实际上还是有点讲究的。前面提到的这种实际上是pdflatex。关于几个相关的一些概念，可以看下这个帖子：

http://bbs.ctex.org/forum.php?mod=redirect&#038;goto=findpost&#038;ptid=56901&#038;pid=377061

另外可以看一下这个总结性的页面，稍微有点老：

[LaTeX to PDF](http://www-rohan.sdsu.edu/~aty/bibliog/latex/LaTeXtoPDF.html)

总的来说，现在用的基本都是LaTeX，不考虑TeX，下面讨论的排版引擎仅限pdfLaTeX和XeLaTeX。这两个排版引擎有各自独立的命令，而在TexWorks当中也可以比较方便地进行设置。

### pdfLaTeX

CTeX套装中默认的编译方式。对于中文，直接使用ctex宏包可以基本解决中文排版的问题。当然也可以折腾比较老的CJK等方法。

对于GB2312格式的文件，引用ctex宏包的时候直接使用\usepackage{ctex}命令。对于UTF8格式的文件，引用的时候加上“UTF8”参数，即

```latex
\usepackage[UTF8]{ctex}
```

**强烈建议使用UTF8格式保存、编译LaTeX文档**，这样可以避免一系列在复制文字、书签等一系列事情上由于编码问题导致的麻烦（这也是我讨厌WinEdt、讨厌CTeX套装的原因之一）。同时，**强烈建议使用Windows自带记事本之外的更专业的编辑器**，记事本对编码、换行的支持很不好。

### XeLaTeX

XeLaTeX产生的原因之一是在编码和字体上进行更好的支持（可以看维基百科的条目），语法本身和LaTeX没有很大区别。根据[这个问答](http://tex.stackexchange.com/questions/11462/does-anyone-know-a-good-xetex-tutorial)，如果有LaTeX的基础，好好学习fontspec就可以掌握XeLaTeX啦。不过对于中文的支持并不是想象的那么好，还需要一些额外的设置，包括间距什么的，这个有很多额外的资料。也就是说，并不是xelatex拿来加一条命令直接就能得到像用pdflatex时ctex宏包那样的效果。

### BibTeX

不知道这个算不算排版引擎，先放这了。不过这里要说的和前面两个没什么关系，也不是来讲怎么用的，只是简单提一下在Tex Live系统中带有BibTeX的文档如何编译。此时不能简单地使用之前的命令行或者直接在TexWorks里面编译完事。需要通过命令行完成，一共4步：

```
pdflatex *.tex
bibtex *.aux
pdflatex *.tex
pdflatex *.tex
```

*.aux是前面一步pdflatex命令得到的产物。如果对bib文件作出过修改，或者文章中引用作出调整，都要运行这4个命令。动手能力强的可以单独写个快捷键什么的。但如果只是删句话这种，只需要后面两步就可以。