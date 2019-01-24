---
layout: article
title: 使用python提取音乐文件内嵌的专辑封面
tags:
  - Uncategorized
  - python
  - 音乐
date: 2014-08-02 21:38:47
intro: 使用mutagen包从mp3和m4a文件中获取的封面图片
---

第一次写有点技术含量的文章。。。这是我第一次使用python写具有实际使用价值的东西，实际上对python的使用还很不熟练，包括if、for这类基础的语句都查了资料。。。

最近在整理一些音乐专辑，（由于我之前放在音乐目录中的Folder.jpg都被WMP给毁了参见[这里](http://social.technet.microsoft.com/Forums/windows/en-US/747b74e2-4f29-42f2-8457-f9513bee5c23/wmp12-destroying-high-resolution-album-art-tips-and-tricks?forum=w7itpromedia)）需要大量地从音乐文件中提取封面出来，虽然说使用foobar2000再定义一个快捷键完成这样的操作也就10秒钟，但实在是太烦了干脆直接写个程序搞定。我的目标就是将音乐文件拖到一个exe上直接生成它的封面，省去了很多中间的步骤。

一开始就考虑使用python来完成，主要因为python有很多可以直接拿来用的库，本身写起来也很方便快捷，而如果用C语言之类完全不知道对于音频图像这些该怎么处理。实际上很幸运，通过搜索很快在StackOverFlow上面的[提问](http://stackoverflow.com/questions/6171565/how-do-i-read-album-artwork-using-python)中找到了[mutagen](https://bitbucket.org/lazka/mutagen)这个库。但是下面遇到了不少麻烦。

<!-- more -->

首先，我以为刚才那个问题的答案中直接将那个mp3文件的文件名改掉就可以直接用，后来发现不对，MP3文件的专辑封面是存储在'APIC:'当中的，而m4a文件并非如此，到底存储在什么地方找了很长时间也没找到。因此考虑换方法，不使用File这个类（**更新：实际上可以使用，见文章末尾**）。但网上只能找到使用mutagen从MP3文件中提取的样例，而对AAC（m4a）的处理几乎找不到任何资料。而它的[文档](https://mutagen.readthedocs.org/en/latest/)感觉也写得不是特别清楚（不知道是不是因为我python本身不熟悉而且文档看得比较少），折腾了好久才明白应使用[MP4Tags](https://mutagen.readthedocs.org/en/latest/api/mp4.html#mutagen.mp4.MP4Tags)这个类来实现。实际上对MP4文件的处理要比MP3麻烦一些。另外文档中只说明了MP4Tags是一个字典，并没有说每个键的值是什么情况，后来才发现对于'covr'这个键对应的值是一个list，图像存在第0的位置。

实际上，这一块试了好久都没成功，一直提示KeyError，我以为是自己的获取值的方法不对，又在网上到处找示例，也没找出来什么东西。后来发现从MP3文件中提取图片也无法成功，同样提示KeyError，才将目标转向python本身。后来发现将python换成2.7就可以解决了。虽然官网的说明是可以在2.6+上使用，但是不知道为什么3.x似乎就是不可以。

更换python版本后问题m4a文件内嵌专辑封面很快就解决了。将MP4Tags.['covr']的那个列表输出发现是很长一串乱码，目测就是图像本身。然后这个列表的长度是1，那么很显然通过MP4Tags.['covr'][0]就可以将图像取出，后面存储的过程就一样了。

之前提到我希望将音乐文件直接拖到程序上就输出封面，目前这样似乎还不太好办，主要是文件无法拖动到.py文件上，建立快捷方式也不行（尽管这些可以通过修改注册表来实现），因此考虑直接输出exe。另外，打包成exe也便于将这个程序分发出去，毕竟不可能让每个人都装一个Python2.7再手动安装mutagen这个包。根据网上的教程，使用pyinstaller来生成exe文件，不过中间都折腾了一下，我发现生成的程序一运行就弹出Windows的“程序已停止运行”，甚至写个Hello World都这样，真是无奈了。之后在虚拟机中的Windows XP上做了一个exe倒是可以运行，但是如果复制到我的Windows 8.1机器上又没法运行了。不得已将pyinstaller的-F开关去掉，放弃生成独立exe的想法，这下倒是可以了。

由于最终是运行exe文件，拖拽上去之后命令行有2个参数，第一个是程序的路径，第二个是音乐文件的路径。我希望将封面输出到音乐文件所在的文件夹下，为了避免路径上出现问题，程序中全部使用绝对路径。很自然封面图片的路径就是将音乐文件路径的最后一个\后面的内容换掉。本来想使用正则表达式替换的，简单又好用，但是python似乎也不支持替换成'$1 - $2'这样的格式，搞了一下没搞出来（水平太烂了-_-），最后只好使用字符串切割和拼接来完成。

写了这么多过程，把代码贴一下吧，顺便给出可以直接运行的exe分发包。代码中使用了异常处理。代码当中有不少重复，由于python写得太少我也不知道怎么写比较好，希望有大神能够指点一下。

```python
from mutagen import File
from mutagen.mp4 import MP4;
import sys;

if len(sys.argv) &gt; 1:
  path = sys.argv[1];
  pathlist = path.split('\\')
  imagepath = ''
  for sec in pathlist[:-1]:
  imagepath = imagepath + sec + '\\'
  imagepath = imagepath + 'Cover.jpg'
else:
  print('No argument')
  raw_input()
  sys.exit()

if path[-3:] == 'm4a':
  file = MP4(path)
  try:
    artwork = file.tags['covr'][0]
  except:
    print('Cover Not Found')
    raw_input()
    sys.exit()
elif path[-3:] == 'mp3':
  file = File(path)
  try:
    artwork = file.tags['APIC:'].data
  except:
    print('Cover Not Found')
    raw_input()
    sys.exit()
else:
  print('Filetype Not Supported')
  raw_input()
  sys.exit()

try:
  with open(imagepath, 'wb') as img:
    img.write(artwork)
except:
  print('Write Error')
  raw_input()
```

分发包下载在[这里](http://pan.baidu.com/s/1o6mcvt4)。

**更新：**之前提到放弃File使用MP4Tags这个类，后来发现其实不用这样，仍然可以使用File类，只要将处理MP3文件时的tags['APIC:'].data替换成tags['covr'][0]就可以了，效果和使用MP4Tags完全一样，这样下来代码可以简单不少。

**更新**：建了一个字典，仅仅使用File类，同时考虑到奇葩大写扩展名的存在，这样下来代码简单了一些，把第17到36行修改之后的部分贴出来吧

```python
ext = path[-3:].lower()
key_mapping = {'m4a':'covr', 'mp3':'APIC:'}

if ext in key_mapping:
  file = File(path)
  try:
    artwork = file.tags[key_mapping[ext]]
  except:
    print('Cover Not Found')
    raw_input()
    sys.exit()
else:
  print('Filetype Not Supported')
  raw_input()
  sys.exit()

if ext=='m4a':
  artwork = artwork[0]
else:
  artwork = artwork.data
```