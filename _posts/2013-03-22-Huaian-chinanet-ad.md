---
layout: article
title: 控诉淮安电信宽带上网推送广告
tags:
  - Uncategorized
date: 2013-03-22 00:26:43
intro: 中国电信劫持用户的网络使用并插入广告。与电信斗智斗勇的过程。
---

这个寒假在家上网的时候经常看到中国电信的广告：

[![](https://ww2.sinaimg.cn/large/005yyi5Jjw1eiq3oaln11j311y0k8n4l.jpg "截图1")](https://ww2.sinaimg.cn/large/005yyi5Jjw1eiq3oaln11j311y0k8n4l.jpg)

[![](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3obyrq5j311y0kgahs.jpg "截图2")](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3obyrq5j311y0kgahs.jpg)

[![](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3oa5j7yj311y0k8gqc.jpg "截图3")](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3oa5j7yj311y0k8gqc.jpg)

现象是输入某个网址出现以上页面，大约十几秒后自动转向原来访问的页面。我在南京使用电脑时从来没有出现过上述网页。分析源代码发现这些内容都来自harx.cn，即“淮安热线”网站。我来复制一段淮安热线[自己的说明](http://www.harx.cn/About/Default-7.html)：

淮安热线（http://www.harx.cn/）是由中国电信淮安分公司投资建设的淮安门户网站，以中国电信强大的主干通信网络为依托，拥有雄厚的综合网络实力，可以满足不同行业的应用需求，信息内容涉及各个领域。淮安热线以淮安为服务区域……

其实大概可以想到这是中国电信做的手脚。中国电信之前在其他地区也有类似的情况。而在淮安，类似的行为也不是第一次，之前是弹窗、嵌入右下角广告，方法一般是直接在网页源代码中插入js脚本。先看一些媒体对其他地区中国电信类似行为的报道：

* [cnBeta.COM-绵阳电信专劫百度新闻，出现巨幅广告](http://www.cnbeta.com/articles/25916.htm)
* [cnBeta.COM-强制弹窗 电信广告现身广州](http://www.cnbeta.com/articles/18575.htm)
* [综合报道:有报告称中国电信劫持http访问，运营ADSL推送式流氓广告业务(图)](http://www.cnbeta.com/articles/14385.htm)
* [中国电信劫持http访问 ADSL强推广告？](http://soft.ccw.com.cn/news/htm2006/20060823_205282.htm)

我很清楚这是什么情况，基本上没有疑问。以前一般是修改源代码加入js，但是现在并没有找到带有harx.cn的相关内容，说明换了方法。某一次广告出现的时候，我用Chrome的Developer Tools查看源代码，发现以下内容：

```
&lt;html&gt;&lt;!-- 15 --&gt;&lt;script language=JScript&gt;&lt;!-- function killErrors(){return true;} window.onerror=killErrors; --&gt;&lt;/script&gt;&lt;frameset rows=&quot;*,0&quot;&gt;&lt;frame src=&quot;http://www.harx.cn/ts/tg/&quot; noresize&gt;&lt;frame src=&quot;&quot; noresize&gt;&lt;/frameset&gt;&lt;/html&gt;
```


那个淘宝热卖就是http://www.harx.cn/ts/tg/的内容。但是看不出来是如何转向到原来访问的网址的。怀疑是在HTTP Header上也做了手脚，于是开始使用抓包软件。比较业余，用的是SmartSniff这个小软件。由于广告是随机出现，而且概率很小，想要捕捉到一次很难。终于守株待兔抓到了这个包。完整的记录在[抓包1](/contents/2013-03-22-Huaian-chinanet-ad/Capture1.txt)。

重点关注以下几行：

```
Host: www.99daili.com
Refresh: 15; URL=http://www.99daili.com/
Cache-control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
&lt;html&gt;&lt;!-- 15 --&gt;&lt;script language=JScript&gt;&lt;!-- function killErrors(){return true;} window.onerror=killErrors; --&gt;&lt;/script&gt;&lt;frameset rows=&quot;*,0&quot;&gt;&lt;frame src=&quot;http://www.harx.cn/ts/tg/&quot; noresize&gt;&lt;frame src=&quot;&quot; noresize&gt;&lt;/frameset&gt;&lt;/html&gt;
```


现在应该很明显了。Host没有问题，是我要访问的网站，但是返回的HTTP Header完全是伪造的，返回的源文件就是刚才开发者工具里面找到的。至于自动转向，原理是里面的Refresh设置为15秒，刷新一下不至于还看到广告，概率太小了。Cache-Control可以看出推送广告设计者的用心良苦。

我之前发现这个问题的时候并没太多想，但是后来每天都有实在讨厌，决定投诉中国电信。之前我在网上看过别人投诉的经历，加上多次投诉过中国移动，有了一些经验。一开始是在微博上投诉的：

[![](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3o850c1j30g709mq4i.jpg "微博投诉")](https://ww3.sinaimg.cn/large/005yyi5Jjw1eiq3o850c1j30g709mq4i.jpg)

江苏网厅不久就回复了。之后我通过私信取得联系，又发了一张截图。当时我还没有收集到JS脚本和HTTP Header这些信息。之后网厅一直没有和我联系。两个星期过去了一点消息没有，电信相关部门也早该上班了，我又发了一封私信警告如果不回复就投诉到工信部，江苏网厅好像有点害怕很快回复我们会处理。第二天我接到了淮安电信的电话。

但是淮安电信显然一点诚意没有。话务员说咨询“资深人员”，让我检查我的浏览器有没有问题。我强调以我的技术我可以很清楚地明白与浏览器没有关系，是网关上作了手脚，到底怎么回事内部的人员心里非常清楚。话务员说再咨询一下再回复。

第二天没有人联系我。第三天也就是今天，好不容易抓到了头信息，自己打电话过去，话务员表示咨询了一下，结论是与他们没有关系。我表示，截图、源代码、Header这些证据我这里都有，而且看到了淮安热线是淮安电信下的一个门户，前面的宽带提速广告很明显表明是电信推送的。我将这些内容发到了话务员的邮箱，之后话务员反馈“资深人员”正在进一步调查，如果有进展下午会回复我。

我一直对电信业的这种推送广告的事情不能容忍，更不能容忍的是电信业（尤其是中国电信）的服务人员经常侮辱客户的智商，不直面问题而用浏览器、病毒这一类的理由企图搪塞、忽悠客户，可能技术水平低一些的客户直接就被骗过去了，以为确实是自己的问题。然而，而可耻的是在用户投诉了，仍然一遍又一遍否认问题的存在，即使在已经有了非常明显、有力的证据的情况下。不知道电信业服务者，尤其是中国电信的脸皮怎么会这么厚。相比之下中国移动好一些，服务并不完善我也投诉过几次，但对待投诉的态度要比中国电信好很多、诚实很多。

不知道下午的回复怎么样，如果仍然不行就直接投诉工信部。其实之前长时间没有回复，早就应该投诉上级了。很快就要去南京了，可能我也无法核实处理结果，但总是要做的。等等吧。

更新：

客服回复已经取消了广告，让我核实。上了一段时间网之后确实没有再次看到广告。2月23日和24日客服都打了电话给我，但是那几个电话我都没有接到。之后去了南京，在2月25日上午接到了客服打来的电话，我表示之后确实没有再看到广告，另外有两个问题：一，能否保证以后也不会出现广告；二，为什么一定要等我将所有的截图、抓包数据拿出来之后才承认有推送广告的情况存在并进行屏蔽？客服回复，不能保证永远不会再出现广告，只能保证最近一段时间内，因为“公司的政策也会变化”；另外，他们确实没有隐瞒，电信企业有规定不允许强制推送广告，他们也是看到了我的这些数据之后才发现这个情况的。对这个回复不想说什么，到底做了什么他们自己清楚，说不知道推送了广告这种话也就骗骗自己吧。有了这次打交道的经历，以后再看到广告就直接投诉工信部，不和本地的通信公司扯皮了。
