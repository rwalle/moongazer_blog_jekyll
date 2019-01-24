---
layout: article
title: 我的LaTeX小结（一）：LaTeX基础
tags:
  - Uncategorized
  - LaTeX
date: 2013-08-30 04:32:44
keywords: LaTeX, CTeX, 公式编辑
intro: LaTeX的基础使用，从Hello World示例开始介绍LaTeX文档的组成和基本命令，进一步介绍数学式的输入、图表的插入以及其它常用命令。
---

[我的LaTeX小结（二）：工具和排版引擎](https://moongazer.me/2015/08/07/latex-notes-ii/ "我的LaTeX小结（二）：工具和排版引擎")

对于搞理科（尤其是数学）的人来说，LaTeX是很重要的工具。如果是要在国外期刊发表文章，肯定是要用到LaTeX的。相比于使用Word+MathType来排版，使用LaTeX排版要好看很多，还有其他一些优势。但是LaTeX这东西有点反人类，没有什么所见即所得（WYGIWYS）的编辑器，学起来花时间，细节的东西处理起来也很麻烦。

都说LaTeX难学，但我觉得LaTeX学起来并没有非常困难，基础的东西很少，只要将基础的掌握了就能上手；然而要想将Latex用好并不容易。按我一位老师的话来说，是“学起来简单，用起来难”。许多LaTeX的教材和我的想法不太一样，我觉得既然TeX是一门语言，就应该安装学习语言的方式来。如果作一些可能不太合适的对比，TeX本身并不是像C、C++这种很严格、成体系的语言，某种意义上有点像Javascript、PHP这类解释型的语言。那么，我觉得可以像很多讲解PHP的书一样，以一个任务为中心，先将最基本的东西做出来，再逐渐将其它内容加入，细节的许多东西可以放在附录之类的地方。而实际上一些书分为布局、表格、数学式、图形等几大章，尽可能地将主要的内容都包含进去，虽然看上去很全面，但是整体脉络不清晰，可能全书看完都不知道如何排一篇文章出来，而且查阅的时候可能还不方便。

言归正传。虽然文章标题是小结，但是这篇文章是想很不自量力地写一个非常基础的教程。虽然现在关于LaTeX的入门教程也很多（如下面的lnotes2和lshort-zh-cn），但我在这里希望用自己的思路简单写一点出来，另外希望自己提供的一些参考资料和建议能够有帮助。我自己学习LaTeX的时候由于没有收到任何他人的指点，走了不少弯路，浪费很多时间，希望能够看到这篇文章的人学起来可以稍微轻松一些。声明一下，本人目前为本科生，虽然用LaTeX排过几篇文章，也有了一定的积累，然而可能对一些基础概念的认识还不够深刻，难免会出问题，如果对本文中的内容或观点有异议，欢迎指出。

首先列一些参考资料。这些都能在网上找到下载。我这里只给出没有版权的资料的链接。

书籍：

*   LaTeX2e完全学习手册 胡伟（第二版已出，[亚马逊](http://www.amazon.cn/dp/B00CGUKVH4 "亚马逊")） 我基本看过全书，这本书真的是一本“手册”，除了比较基础的内容需要仔细看以外，其它可以只看大概，具体用到的时候再查。第一版书中有一些小问题，不知道第二版有没有更正。
*   LaTeX2e科技排版指南 邓建松 这本书有点老了，现在应该买不到全新的，但是图书馆一般能找到
*   LaTeX入门 刘海洋（[亚马逊](http://www.amazon.cn/dp/B00D1APK0G/)） 今年6月刚出的书，不太了解，貌似评价不错，可以看看
*   [The TeXBook](https://pdfsizeopt.googlecode.com/files/texbook.pdf) Donald E. Knuth 经典书籍

相关的资料、文档（有些虽然和书差不多，还是归为文档吧）

*   **[lshort-zh-cn](http://www.ctan.org/pkg/lshort-zh-cn)** （一份不太简短的 LATEX 2ε 介绍）
*   **[lnotes2](http://bbs.ctex.org/forum.php?mod=viewthread&amp;tid=43774)**  （雷太赫排版系统简介） 包太雷 前面的链接需要注册才能下载，我这里也发一份：[lnotes2](/contents/2013-08-30-latex-notes-i/lnotes2.pdf)
*   [Math Mode](http://www.tex.ac.uk/ctan/info/math/voss/mathmode/Mathmode.pdf)  Herbert Voß* 关于数学式的非常详细的说明
*   [typeset](https://code.google.com/p/chenshuo/downloads/detail?name=typeset.pdf) （用 LATEX 排版编程技术书籍的一些个人经验） 陈硕
*   [symbols-a4](http://www.tex.ac.uk/tex-archive/info/symbols/comprehensive/symbols-a4.pdf) （The Comprehensive LATEX Symbol List）Scott Pakin 各种可能用到的数学符号在这里都能找到
*   同济：[LaTeX命令速查手册](http://www.tongji.edu.cn/~yin/cn/latexfast.html)

**另外关于LaTeX的使用有一个很重要的提醒：**现在互联网很发达，如果在LaTeX的使用中对于符号、排版等等遇到疑问，多使用搜索引擎寻找答案，使用清晰的关键词，如果互联网上有答案但不够明确再去查阅资料。一般来说遇到的问题大多已经有人问过，至少我的情况基本是这样。如果没有答案，可以使用英文关键词搜索英文网页；仍然没有可以再去一些论坛求助。可以多关注[ChinaTeX论坛](http://bbs.chinatex.org/forum.php)和[CTeX社区](http://bbs.ctex.org/forum.php)。

## 基础教程

### Hello World!

下面就开始这个简单的教程了。<del>首先[下载CTeX套装](http://www.ctex.org/CTeXDownload)，建议下载完整版，用得省心，虽然有点大。</del>（当时真是too young too simple……不建议使用CTeX套装，具体请看[我的LaTeX小结（二）](http://moongazer.me/archives/121 "我的LaTeX小结（二）：工具和排版引擎")）下载安装完成之后，打开WinEdt软件，图标如下：

<div id="attachment_352" class="wp-caption aligncenter" >[![WinEdt](http://ww4.sinaimg.cn/large/005yyi5Jjw1eiq415sgd7j30740740sx.jpg)](http://lizhe.co/wp-content/uploads/2013/08/winedt.png)

WinEdt
</div>

打开之后，新建文档，写一个"Hello, World!"的例子：

```latex
\documentclass{paper}
\begin{document}
Hello, World!
\end{document}
```

保存文档，按下F9进行编译，显示效果如下所示：

[![tex-helloworld](http://ww4.sinaimg.cn/large/005yyi5Jjw1eiq412pf6kj30da02at8i.jpg)](http://ww4.sinaimg.cn/large/005yyi5Jjw1eiq412pf6kj30da02at8i.jpg)

简单解析一下：\begin{document}和\end{document}

是正文所属的**环境**，所有的环境都要以

```
\begin{}
```

开始，以

```
\end{}
```

结束。在

```
\begin{document}
```

前面的都是对文档的一些说明，

```
\documentclass{paper}
```

表明文档的类型是一篇论文，除了paper之外还有book、letter等等，类型的不同会导致版式不同，章节层次等等也会不一样。

LaTeX默认是不支持中文的，如果要使用中文，可以在\begin{document}前加上：

```latex
\usepackage{ctex}
```

\usepackage{ctex}表示使用**宏包**ctex，宏包中预定义了一些内容，如果要实现一些额外的功能，如插图、更改页面样式、使用特定字体等等都需要\usepackage来引入宏包。

关于实现对中文的支持，还有一些方法如使用\documentclass[CCT]{ctexart}和\documentclass[CJK]{cctart}等等，很多书也是用这些的，但是在字体等方面的支持可能有点问题，而如上所示的使用ctex宏包就没有问题。所以我基本上都是用\usepackage{ctex}的方法。

### 数学式

现在加入数学式：

将Hello, World!换为：

```latex
\begin{equation}
    E=mc^2
\end{equation}
```

即得$E=mc^2$。注意：实际编译得到的结果后面会有一个编号(1)，关于这类序号的编号有非常灵活的应用，可以查阅相关资料。

这里使用了**公式**环境：equation。注意普通的文字放在外面，公式放在公式环境里面。这是普通的单行公式环境，如果要实现多行、对齐等等效果，还要结合align、array等环境。^表示上标，类似的还有_表示下标。注意只能接一个字符，如果上标和下标有多个字符，需要用花括号{}括起来，如：

```
A_{12}
```

显示为$A_{12}$

分数的表示：

```
x=\frac{a}{b}
```

结果为$x=\frac{a}{b}$

\frac{}{}是分式命令，后面两个括号内是它的两个参数，分别是分子和分母。

这里各种可以任意嵌套，只要括号之类的嵌套层次正确就行，如：

```latex
F_{\mu\nu}=\frac{\partial A_\nu}{\partial x_\mu}-\frac{\partial A_\mu}{\partial x_\nu}=F_{23}
```

显示为：

$F_{\mu\nu}=\frac{\partial A_\nu}{\partial x_\mu}-\frac{\partial A_\mu}{\partial x_\nu}=F_{23}$

这里出现了偏导符号∂，以及μ、ν这样的希腊字母。一般来说，这类“特殊符号”需要宏包amsmath，即在\begin{documentclass}之前要加上：

```latex
\usepackage{amsmath}
```

amsmath含有常用的一些字母、符号，某些符号需要其它的宏包，查阅资料的时候一般会给出。

这些符号不用可以去记，一开始调用WinEdt菜单的TeX - CTeX Tools - TeXFriend... ，会出现这样的窗口：

[![TeXFriend](http://ww1.sinaimg.cn/large/005yyi5Jjw1eiq4133ckkj309n0hignb.jpg)](http://ww1.sinaimg.cn/large/005yyi5Jjw1eiq4133ckkj309n0hignb.jpg)

直接点击即可插入到光标所在位置，很方便。当然如果每次都点肯定就麻烦了，用多了这些字幕、符号对应的命令应该都能记住。这里∂的命令是\partial，μ的命令是\mu，ν的命令是\nu。希腊字母的命令很简单，大写字母的命令就是反斜杠\加上英文读音首字母大写的结果，小写则加上读音全部小写，如Σ是\Sigma，σ是\sigma。**这里注意：\sigma之类字母、符号的命令是一个整体，显示出来的是一个字母或符号，因此在上下标之类的地方不需要加花括号{}。**

一般来说用到最多的也就是分数、上下标、字母与符号了。

对于公式和后面要提到的插图、表格，往往会在文章中其它地方引用。在LaTeX中这几种内容的引用方式都是一样的。在被引用的地方加入\label命令，在引用的地方加入\ref命令。比如

```latex
\int_a^b f(x)dx=F(b)-F(a)
\label{eq:Newton_Leibniz_Equation}
```
\label后面的花括号为引用名，可以自己填上一个有意义的、唯一的名称；这里的eq表示公式，类似地在插入中可以使用fig等；在需要引用的地方，这样使用：
```
根据式(\ref{eq:Newton_Leibniz_Equation})……
```

括号里面的内容会自动替换成序号。这个序号需要两次编译后才能出现，具体的可以参见相关资料。

### 插图

首先引入graphicx这个包。在需要插图的地方，使用以下命令：

```latex
\begin{figure}
    \centering
    \includegraphics[width=6cm]{fig1.pdf}
    \caption{第一张图}
    \label{fig:fig1}
\end{figure}
```

即可插入。此处width可以换成其它参数，cm这个单位也可以修改。在pdflatex中可以使用png和pdf等格式，jpg格式请转换后插入。**一定要注意\**caption命令在\label命令前面，这对于表格也是同理。 如果关于一个主题有多张图，如不同参数下的对比，可以使用subfigure。首先引入subfigure包，代码示例

```latex
\begin{figure}
    \subfigure[Caption] {
        \includegraphics[width=cm]{.pdf}
        \label{fig:}
    }
    \subfigure[Caption] {
        \includegraphics[width=cm]{.pdf}
        \label{fig:}
    }
\end{figure}
```

关于图文混排，可以使用wrapfig包。图文混排在正式的论文中不多见。

### 表格

表格可以使用tabular包。先放代码：

```latex
\begin{center}
    \begin{tabular}{|c|c|c|c|} \hline
    元素 &amp; 道址 &amp; 线系 &amp; 特征X射线的能量(keV) \\ \hline
    Cu &amp; 216 &amp; $K_{\alpha1}$ &amp; 8.04778 \\ \hline
    \multirow{2}*{Pb} &amp; 291 &amp; $L_{\alpha1}$ &amp; 10.5515 \\ \cline{2-4}
    &amp; 347 &amp; $L_{\beta1}$ &amp; 12.6137 \\ \hline
    Mo &amp; 483 &amp; $K_{\alpha1}$ &amp; 17.47934 \\ \hline
    Fe &amp; 167 &amp; $K_{\alpha1}$ &amp; 6.40384 \\ \hline
    Zn &amp; 232 &amp; $K_{\alpha1}$ &amp; 8.63886 \\ \hline
    Y &amp; 415 &amp; $K_{\alpha1}$ &amp; 14.9584 \\ \hline
\end{tabular}
\end{center}
```

效果如下所示 ![](http://ww3.sinaimg.cn/large/413ed6d6gw1eutt7t6sc3j209m052wev.jpg) center表示居中。{tabular}后面花括号里面的内容这样理解：c表示居中，一般来说表格中内容应该也是居中，可以自己换成l或者r；|表示分割；c的个数表示表格的列数。也就是说通过这个花括号里面的内容来决定了表格的外观。后面每一行中通过&amp;来分割，如果表格的某一行某一列为空，那么空着就行，即a &amp; b &amp; &amp; c，这和公式里面的对齐是一样的；\\表示换行；\hline表示表格划线。 如果要实现跨行表格或者跨列表格，需要用到multirow包。在上面的表格中，Pb跨了2行，multirow后面的花括号里面的数字就为2；由于跨了一行，那么久不可以用\hline命令划整行的线，因此使用\cline命令，只在第2-4列下划线。 关于跨行、跨列表格，可以再看一个例子（转载）：

```latex
\begin{tabular}{|c|c|c|c|c|}
\hline
\multirow{2}{*}{Multi-Row} &amp;
\multicolumn{2}{c|}{Multi-Column} &amp;
\multicolumn{2}{c|}{\multirow{2}{*}{Multi-Rowand Col}} \\
\cline{2-3}
 &amp; column-1 &amp; column-2 &amp; \multicolumn{2}{c|}{} \\
\hline
label-1 &amp; label-2 &amp; label-3 &amp; label-4 &amp;label-5 \\
\hline
\end{tabular}
```

效果如下所示：

![](http://ww1.sinaimg.cn/large/413ed6d6gw1eutteybofoj20b9026glp.jpg)

可以参照我上面的说明理解。 一般的论文写作中主要就是公式、插图、表格，因此我想如果能搞定以上这些，已经能解决80%的问题了。

## 幻灯片的制作

对于公式较多的内容，如果使用PowerPoint制作幻灯片，文字和公式的混排很麻烦，并且在转移到其它电脑时公式经常会乱掉。LaTeX做出来的幻灯片是比较美观的，简洁大方，不过做的时候不够直观，需要反复调整。贴一张图： [![LaTeX幻灯片示例](http://ww3.sinaimg.cn/large/005yyi5Jjw1eiq4147hh3j30s00l0q4z.jpg)](http://ww3.sinaimg.cn/large/005yyi5Jjw1eiq4147hh3j30s00l0q4z.jpg) 简单提一下幻灯片的制作。\documentclass后面的paper改为beamer，比如：

```latex
\documentclass[serif]{beamer}
```

serif表示使用serif字族，这么做是因为幻灯片默认的字体不是很好看。后面可以使用\usetheme{}来选择模板，上面图片中使用的是Madrid。各种模板的预览可以到[这个网址](http://deic.uab.es/~iblanes/beamer_gallery/index_by_theme.html)看一下。

在book或者paper中，我们不需要考虑分页的事情，但是beamer中分页很重要，需要自己把握好。每一页是一个frame环境，中间可以用\frametitle{}加上标题，一个简单的例子是

```latex
\begin{frame}
    \frametitle{不确定性关系}
    \begin{equation}
        \Delta p \Delta x \geqslant \frac{\hbar}{2}
    \end{equation}
    \begin{equation}
        \Delta E \Delta t \geqslant \frac{\hbar}{2}
    \end{equation}
\end{frame}
```

如果不想每一页全部一下子展示出来，希望演讲者来控制每一行出现的时间，可以在需要的地方加上\pause，这样原来同一个分页中的内容会生成多个PDF，对于上面的例子，如果在两个equation中加入\pause，效果就是这样：

[![example_2](http://ww4.sinaimg.cn/large/005yyi5Jjw1eiq41540x4j30s00l03zp.jpg)         ](http://ww4.sinaimg.cn/large/005yyi5Jjw1eiq41540x4j30s00l03zp.jpg)[![example_3](http://ww1.sinaimg.cn/large/005yyi5Jjw1eiq413rod7j30s00l0myg.jpg)](http://ww1.sinaimg.cn/large/005yyi5Jjw1eiq413rod7j30s00l0myg.jpg)

起到翻页的效果。如果希望做完演示再分发讲义，即忽略\pause，去掉重复的内容，那么可以在\documentclass后加[handout]，如

```latex
\documentclass[serif,handout]{beamer}
```

这样就只会生成上面实例中第二张内容的PDF。

关于幻灯片主要内容就是这些，其它内容基本上与paper中类似。关于beamer其它一些应用可以参考相关资料。

本人只想写一个很基础的教程，不打算继续写高级的应用，也没有这个水平。前面这部分如果我想到了会继续完善，也欢迎提出意见。

各种零碎的提示：

*   注释：一行内%符号后的所有内容都为注释
*   换行：使用\\换行
*   微分算子d的输入（非斜体）： 可以在文档开始之前定义
```latex
\def\mathd{\mathrm{d}}
```

需要插入d的时候，输入\mathd，如

```latex
\frac{\mathd x}{\mathd y}
```

即显示为$\frac{\mathrm{d}x}{\mathrm{d}y}$

*   $\left. [\delta y] \right|_{x=x_0}=0$的代码：
```latex
\left. [\delta y] \right|_{x=x_0}=0
```

*   使用ctex包，图像和表格序号前面的文字为英文，如“Figure 1”，可以通过以下命令修改：
```latex
\renewcommand{\figurename}{图}
\renewcommand{\tablename}{表}
```

&nbsp;

想到了再加吧。

========================================================

附上我自己最近使用LaTeX写的小论文和演示。**注意：这几个文件仅作为语法上的参考，本身很不完善，有Warning和Bad Box，不要当作例子去学习。可以到网上找其他人做的比较规范的文档来学习。另外里面的内容水平并不高，大家看了不要见笑。**

[kkrelation](/contents/2013-08-30-latex-notes-i/kkrelation.tar.gz) [up](/contents/2013-08-30-latex-notes-i/up.zip)
