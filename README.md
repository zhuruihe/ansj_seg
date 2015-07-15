Ansj中文分词重构项目
==================
目标: 
* 升级到目前最新的maven-3.3.3
* 在不改变原来代码逻辑的前提下进行重构
* 消灭 过多的 全局性的 static 变量
* 清理 精简 代码使之更易读切不易产生bug
* 更多地使用值对象等不可变对象
* 改善字典重加载

打包发布:
分词器
    
    mvn clean compile package deploy;

Lucene插件

    cd plug; mvn clean compile package deploy;
    
重构动机:
* 非常喜欢ansj分词器, 它基本能达到预期的效果
* 但是代码风格不够好, 包括nlp-lang包
* 在性能够用的情况下, 代码的可读性优先于性能
* 不好读的代码不易维护, 容易隐藏bug或导致用户使用不当引发bug
* 这么好的一个库, 代码不应该仅仅实现功能, 应该便于更多人参考或学习
* 过一遍手完全搞懂它的算法

#####github as mvn-repo
see: http://www.cnblogs.com/lhfcws/p/3503707.html
see: https://github.com/github/maven-plugins
see: http://stackoverflow.com/questions/14013644/hosting-a-maven-repository-on-github

POM方言 https://github.com/takari/polyglot-maven

Ansj中文分词
==================


#####使用帮助[http://nlpchina.github.io/ansj_seg/](http://nlpchina.github.io/ansj_seg/)

#####在线测试地址<a href="http://demo.ansj.org">http://demo.ansj.org</href>


##### 摘要
> 这是一个基于google语义模型+条件随机场模型的中文分词的java实现.

> 分词速度达到每秒钟大约200万字左右（mac air下测试），准确率能达到96%以上

> 目前实现了.中文分词. 中文姓名识别 . 用户自定义词典

> 可以应用到自然语言处理等方面,适用于对分词效果要求高的各种项目.


#####  下载jar
* 访问 [http://maven.ansj.org/org/ansj/](http://maven.ansj.org/org/ansj/) 最好下载最新版 ansj_seg/
  * 如果你用的是1.x版本需要下载[tree_split.jar](http://maven.ansj.org/org/ansj/tree_split/)。
  * 如果你用的是2.x版本需要下载[nlp-lang.jar](http://maven.ansj.org/org/nlpcn/nlp-lang/)
* 导入到eclipse ，开始你的程序吧


#####  maven
1.  使用git下载本项目：

```
git clone https://github.com/NLPchina/ansj_seg
```

2.  进入ansj_seg目录，使用maven安装项目：

```
mvn clean install -Dmaven.test.skip=true
```

3.  在dependencies标签中粘贴如下:(其实version 以最新的为标准.)

````
    <dependencies>
        ....
        
        <dependency>
            <groupId>org.ansj</groupId>
            <artifactId>ansj_seg</artifactId>
            <version>1.41</version>
        </dependency>
        ....
    </dependencies>
````

#####  调用demo

如果你第一次下载只想测试测试效果可以调用这个简易接口

<pre><code>
 String str = "欢迎使用ansj_seg,(ansj中文分词)在这里如果你遇到什么问题都可以联系我.我一定尽我所能.帮助大家.ansj_seg更快,更准,更自由!" ;
 System.out.println(ToAnalysis.parse(str));
 
 ﻿[欢迎/, 使用/, ansj/, _/, seg/, ,/, (/, ansj/, 中文/, 分词/, )/, 在/, 这里/, 如果/, 你/, 遇到/, 什么/, 问题/, 都/, 可以/, 联系/, 我/, 房/, 我/, 一定/, 尽/, 我/, 所/, 能/, ./, 帮助/, 大家/, ./, ansj/, _/, seg/, 更/, 快/, ,/, 更/, 准/, ,/, 更/, 自由/, !/]
</code></pre>




----
##大事记要
#2014年10月10日
> 命运无常,上天和我开了个玩笑,可能我以后没有精力来维护ansj_seg了.所以我把它贡献到nlp_china这个组织中,也许会有有兴趣的同学能替代我维护这个项目.不要乱猜,我身体很好.工作也不忙.感谢你们的支持ansj_seg才能发展这么好.最后祝好.

#2014年6月13日
> 额，今天是黑色星期五。正在紧张而有序的在做ansj2.0版本的升级。如果你用的版本是2.0x都是预览版。不保证稳定性。所以非版本控。不要跟着更新，这次修改的内容主要有：
* 修复对reader流偏移量的错误。主要是因为\n\r造成的不可读错误
* 修复term类的get方法乱用的错误。主要是为了让term对象更好的被json序列号
* 对核心词典的重构。
* 放弃tree-split。jar 迁移到nlp-lang.jar中。nlp-lang是我的一个新项目。内容多多大家又兴趣可以到[https://github.com/nlpchina/nlp-lang](https://github.com/nlpchina/nlp-lang)去看看。
* 重构crf模块。让每个人可以训练自己的crf。补充文档。

#2014年3月10日
> 此次更新，对tree-split中潜伏了n久的偏移量错误进行了修正。之所以修改这个是要作关键字标红。所以理所当然我把摘要做了，同时对关键词抽取作了一些补充了规则。目前摘要还处于alaph阶段。说白了，光一个摘要都能写一篇博士论文，对于开放场景的摘要其实未必需要高深的算法，这个可以在工程中用，能用，简单，但是无法做到行业顶级，每一个应用如果要做细作精，必须去结合自己的需求，以及业务来作。所以这些基础件理念是用20%的工作完成80%的功能。突然想起一句广告词。好不好看疗效。俄。。。写到这里发现走题了。总结下。这次新增了 。摘要 ，基于query的摘要 ，文章标红，优化了关键词抽取。等功能。很想做一件事情，做一个开源的nlp处理工具包。包括摘要，关键词抽取，倾向性分析，主题发现等功能，不在这里做了另起一个项目。有兴趣的可以联系我。加油

#2014年2月10日
> 终于把文档补全，因为时间比较仓促。本人比较懒，也不喜欢写字。所以断断续续的，大致也有了个好的结果。比较让人高兴的是我找到了一个写文档的方式。之写markdown文件。然后用ajax调用marked进行渲染最后通过hightlight插件。对code标记。:-)全部js搞定。也得力于上面两个优秀的开源项目，妈妈再也不担心我的文档了。:-)。对了利用过年这几天，分词做了大量的改动。经过多次的内心挣扎，放弃了一些分词结果较好的办法。将分词程序控制到了50m一下。对于不需要新词发现的用户。比如做搜索建议用ansj_min版本。只有4m左右大小。方便移动节能环保。

#2014年1月21日
> 增加了crf模型的解析。用crf来做未登录词的识别。取得了不错的效果，增加了对长词的进一步解析。将颗粒度防到最低。但是随之而来的影响造成了。分词jar包过大。大约有500多m，无法很顺利发布到git 和 maven库中。试了oschina的maven库也是不可以。如果没有很好的方案。ansj决定放弃maven支持。对于这方面需求的朋友只能说非常抱歉了。我不想因为担心项目的庞大。而畏首畏尾。当然对于jar包的发布可能选择云盘的方案。对于用于搜索的朋友。不建议跟着更新。因为index分词没有作更多的改变。祝好。剩下今年的时间（阴历），有下面几个打算。重构代码。优化里面的关键性算法。完善文档。随缘

#2013年12月12日
> 把由字构词的方式加到了分词中，对未登录词有了很大的提高。对外国人名的识别做了特定的优化。目前正在测试中。新增了httpserver 的控制台。可以直接方便调用分词结果

#2013年9月26日
> 我更新完了发表此帖为止的一次更新。在核心辞典上作了一些手脚。这个版本更像以前的版本。在分词的颗粒度上保持了优良的传统。尤其是面向搜索的用户。一定要更新

#2013-08-28
> 经过无数网友的抗议。ansj终于支持了maven。在这里感谢帮我把项目转换到maven的那个兄弟。你qq我找不到了。名字我也忘记了。

#改进
> 断断续续修改了无数个版本。在csdn的搜索系统上。用12年的历史数据.检索分析等.ansj经受住了考验。但是根据网友和自己的发现。找到了项目中的很多不足于是。开工。。。。。
> 同时在改进的过程中。我认识了更多的朋友。太多了。恩还有在读这篇文章的你。感谢你们对这个小工具的支持。在这里不一一例举了。主要找你们的名字比较麻烦。而我有是个很懒惰的人

#崩溃
> 如大多数的开源者一样，项目带来了很多负担

> 比如。在你工作或者思考的时候。别人就会打断你的思路。qq or email 提出了数个问题。或者bug。当然这些中大多都是友善的很有意义的建议。一方面让我更加坚定做好这个开源分词的决心。另一方面也给我的工作生活带来了一些效率上的影响。大多数提问我都是会回答。而且尽可能的保持耐心。但是如果有怠慢的地方。我在这里对大家表示歉意。

#诞生
> 2012-9-7 日Ansj中文分词。在我整整一夜的奋斗中终于完成了，真的是一夜的奋斗。写着写着一抬头天亮了。当然中间的快乐与心酸这里就不牢骚了。

> 通过微薄@了52nlp希望他能帮我推广下。在他的帮助下。ansj结识了很多朋友。@完后我就去睡觉了。辗转的一个夜晚。当下午醒来的时候。很多人微薄@我。我开玩笑的和cq说。我火了。

> 同时也@了我的启蒙导师张华平老师。他对我表示了支持。在这里感谢他
