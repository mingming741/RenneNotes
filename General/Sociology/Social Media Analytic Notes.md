# Lecture 1: Basic
Social Network：社会网络（或者社交网络），表示，一种独立个体相连接的结构，人用Node表示，关系用Edge表示。

Social Media：通常指的是share content的media，比如youtube，微博这种，侧重于是信息的产生和消耗。而传统的媒体，比如电视，主要是信息消耗，而没有user自己产生信息这一说。侧重一种交互，而不是单方面信息的传递。

通常，social science的研究方向是，人与人之间怎么交互，人如何被影响，信息如何被构筑成网络，为什么有些topic能有巨大流量。而Social engineering研究的，通常是构筑一个社交网络，并且为网络中的user服务。

Social Media Analytic：研究个体和社会的行为，用户的喜好，人们的情感，人们在社交网络上的言论，信息在社交网络上的传播，通过这些信息，构筑实用的service去为其服务，同时推测人们的行为和想法。

### Terms
* Logic
* Analysis
* Reason
* Insight：洞察力
* Intuition：直觉
* Creativity
* Data Mining
* Statistical Model
* Algorithm
* Human Judgment：人为判断
* Processing Power
* Human Cognition：人类认知
* Interpretation：解释，理解
* Data Virualizing

### Measurement
通常，人们的行为，包括社会行为，是通过刺激物(stimulus)产生的反应(response)，例如当人们看到一个捐赠logo的时候，我们决定要捐赠，那么这些概念是可以被测量的：捐不捐，捐的话捐多少，人们的反应时间，总共收集的钱，平均每个小时获得的捐赠，平均每个小时有多少人捐赠...用这些指标，去衡量这个logo设置的好不好，能不能引发人们的捐钱兴趣。通常不同的情景有不同的measurement，反应出了实验的设计步骤。

上面所说的，是我们可以measure的，但是我们其实要分析的，是：为什么他们要捐钱，或者为什么不捐？捐钱的动机，为什么捐这些数量的钱？人们的想法是无法measure的，只能analytic。

而这种measurement通常被分成看(see), 说(say), 感觉(feel), 做(do)四个metric，看的信息是我们提供的，即暴露在环境下(exposure environment)，比如说logo。而说和感觉通常是来源于user的参与(engagement)。最后的做就是user的action，或者behavior，是我们可以measure的。而这四个metic有多种的measure的方式，比如：
* see：user自己网站的流量，关注者，收藏者。例如一个微博博主的粉丝有多少。这个metric用于衡量一个user的社会曝光程度
* say：user喜欢的，转发，回复的内容，主要体现了用户的参与。例如一个微博博主平时的发博和转发，还要回复等等
* feel：user的感觉，通常指带感情色彩的评论，例如微博下面撕逼，吹水这种现象。
* do：对user的影响，例如捐赠，贩卖等等，例如用户在看到卖家的微博之后，去卖家淘宝店逛了逛这种。

### Twitter
通常Social Media Analytic都在这种社交渠道上进行，比如推特。推特的简单形式就是140个字之内的一段文字配上图片或者视频(metadata)等。若使用twitter对社交媒体做严格分类的话，通过下面的mapping可以构建不同的模型：
* follow：反应社会网络情况，可以用有向图表示
* reply & mention：反应communication pattern，谁在和谁说话
* retweets：表示信息的传播(diffusion)


# Lecture 2 Context Analysis

### Human Cognition
Definition：使用知识和理解，去处理思考，感受，和感觉的精神过程，叫做**认知**。其来源为拉丁文的cognoscere，源意思为学习，getting to know。我觉得认知是可以概括三观这个概念的，比三观更加宏大的一个概念。

例如，同一张图片，不同人看到了有不同的理解，比如说里面存在的隐藏pattern。（例如九张脸的图片），不是每个人都能够发觉的到，而发觉到的程度，可以理解为这个人对这张图片的认知。或者说，当你看到猫爪的时候，就会想到猫，你看到卡通的猫的图片的时候也会想到猫，这种联想也是一种认知。对下面的图片。两个词的中间的意义不明的单词，第一个词会联想到THE，而第二个词会想到CAT，但是中间的字符其实是一样的，这就是认知产生的误差。同一个字符，产生的认知不同导致的。

<img src = 'https://github.com/mingming741/RenneNotes/blob/master/Sociology/figure/THE-CAT.png'/>

上面所总结的，都是人类认知物体的一些通性，和机器学习一样，人类认知物体，也是认知物体的feature。我们的记忆中有feature pattern和object的mapping，最后判断出物体大概率属于哪一类。这和机器学习的概率模型非常相似。

### Human Information Processing
外在世界的signal会不断的向我们的大脑传递，而我们的working memory会决定是否要使用这个signal，如果使用的话，这个signal会变成我们要处理的information，并且改变我们的mental state。

同时information之间又是相互关联的，例如说道红色，你会想到颜色，黄色，也会想到苹果，玫瑰。这种关联性被叫做**schemas**

### Karl Popper的三个世界
* World 1： physical object，表示物理意义存在的东西，例如石头，星星，辐射，等自然存在的东西
* World 2： mental或者心理学的世界，通常是人的感受，比如说感觉，想法，感知，通常是主观上的感受。
* World 3： 人类思想的产物。比如语言，故事，数学，歌曲，绘画，等等。
通常，World 3的东西是虚构的，但是可以用world 1的object去表达出来。可以认为，world 3是World 1和2的某种链接。而world 2的思考有来源于我们World 1的大脑。

### Signified & Signifiers
Signified指的是某种确切的mental concept，例如clock，这个词它代表时钟，但是它不明确的指定任意的一个时钟，而是“时钟”这个概念本身，代指了时钟这一类的物体。

Signifier指的是represent了signified的一种表现形式，例如“clock”这个概念，我们可以用clock这个词来表示，也可以用一幅画来表示。

### Social Evidence
Alvin Goldman将人们视为信念(doxastic)的agent，每个人都有内在的信念状态。而内在的信念状态会被social evidence从外在表现出来。我们所能够观察和分析的，只有social evidence，例如我们通过blog post去分析一个人的想法，post就是Social Evidence，而想法本身就是doxastic agent的doxastic state的部分表现。

### Contextualization
内容化，即将抽象的概念具体化。这也是我写笔记的时候潜意识用的最多的方式。例如下面的问题
1. 给出一些卡片，两边一边是字母一边是数字，给出规则“元音字母(AEIOU)的背后移动是偶数”，给出四张卡片的一面：4，A，L，7。我们需要翻哪两张卡片来verify这个rule？ （答案应该是A和7，思路是充分条件和不必要条件）
2. 给出一个情景，乘搭公共交通的人都需要票，我们如何验证这个rule？给出的可以验证的有：“有票的人”，“使用公共交通的人”，“没有使用公共交通的人”，“没有票的人”。很容易去想到用2和4去验证。

上面的过程叫做Contextualization。在概念被具体化到生活场景中去的时候，我们会发现没有改变的概念本身变得更好理解了。（这也是学习和教育工作的一个手段）。但是对于计算机来说，计算机无法理解内容化的东西，所以我们在使用计算机做analytic的时候，需要decontextualized我们的social内容。（这也是人类语言和计算机语言的一个gap）


# Lecture 4 Social Media Psychology(心理学)
Key Idea：
1. 行为是可以被reward和punishment改变的。
2. 认知影响行为
3. Social situation会shape我们的behavior。（即我们的行为会因为社会地位的不同而不同）

而社会心理学social psychology，是通过social context去分析个体的行为。我自己的理解是，通过群体或者个体现象，了解群体或者是个体的某些特性，而得出结论。首先要确定研究对象，再是渠道，再是研究内容，再是结论。例如，我们分析香港人对于香港房价的态度，香港人是研究对象，facebook可以是研究渠道，然后内容是放假，我们通过收集和分析facebook上面关于香港人对放假的评论和发帖，做出统计，然后得出结论。再例如，我要分析我喜欢的妹子的爱好，妹子就是对象，渠道可以是空间，朋友圈或者微博，然后我通过收集她分享评论转发的内容，分析出来她对什么方面感兴趣。这种分析在日常生活中也非常的普遍。

### Theory of Planned Behavior
Behavoir来自于intention，而这种intention的behavior被下面的三个因素影响：
1. 对这个behavior的Attitude（通常是一种信念），例如我对中大学生游行这件事本身的态度
2. Subjective norm（自身规范），对于游行这件事情，我有我自己的规范，就是不参加这类的聚众活动
3. Perceived behavior contorl：察觉自己能否有效进行这件事情，即我参与这种活动能否达到某种目的。

总结起来，就是态度，规范，和可行性。在你觉得这件事情是积极的，并且这件事符合你的行为规范，并且可行的时候，你可能就去做这件事情。所以也叫planned behavior。

### Uses & Gratification(满意) Theory
解释了people用social media去满足到他们什么样的需求，例如：
1. Cognitive needs：认知上的需要，例如收集信息，生存，理解周围的环境。例如刚来香港的时候，洞察周围情况。
2. Affective needs：通常上是情感上的体验，aesthetic(美学的)。例如看可爱的小姐姐，好看的cosplay
3. Personal Integrative（综合的） need：建立自信，可靠程度。例如秀恩爱秀旅游秀工作等，也是满足自己的虚荣
4. Social Intergrative need：和朋友以及家人建立联系
5. Tension-release needs：escapism，diversion，即逃避，减少压力，沉迷在虚拟世界的作用。

这个理论说明了个体在使用social media都是出于满足自己的某种需求。我们在分析一个人的post的时候，可以尝试去分析一下ta使用post的动机，大概就是上面几类中的一类了。

### Social Presence Theory
media的Social presence都是不一样的，并且这种social presence会使得两个partner会有交互。越强的social presence会让两个partner互相之间的影响越强。例如，面对面的social presence大于电话，大于live chat，再大于email。这也是**Media Richness Theory**的内容。通常reduce uncertainty是增强一个media的social presence的重要内容。

这理论说明不同media对交互两方的影响程度不同。

### Self-Presentation
即任何social interaction中，参与者都会有desire去控制别人对自己的impression。例如我们的post，就是再给我们的朋友塑造我们自己想要在他们眼中的形象。

这么总结了一下，倒反许多人在微博中塑造的形象其实只是他们内心中自己的形象，并不一定和真实的自己一样。但是至少这是他们内心的样子，所以找对象还是找个喜欢二次元喜欢cosplay的妹子吧。

### Self-Disclosure
Self-Presentation是Self-Disclosure完成的，即有意识和无意识的自我揭示，比如我们平时的想法，感觉，喜欢和不喜欢，这些都是表现在别人的眼中的。因为我们有自己的Self-Presentation，所以我们会控制自己的Self-Disclosure去表现。

可以任务Self-Disclosure是一种自我表现，用于构筑一个人的Self-Presentation

### Honeycomb（蜂巢） Model （Kietzmann）
表现了social media的7个functional block。用于描述不同的user experience的specific facts。包括：
1. Identity(中心block)：user自己选择的，对自己的identities表现的social media setting。即网名，签名，位置，性别，还要你想要share给别人看的信息，可以理解为user自己对自己的定义。
2. Group：这个group表示user对于自己分组信息的一些要求，并且研究表面，一个user最多同时maintain不超过150个其他人，无论如何分组。而一个用户对其他用户的分组，体现出了network的概念。
3. Relationships：用户之间的relation，可以用箭头来表示，例如关注和被关注。
4. Reputation：表示一个user被关注的程度，比如facebook被点赞的数目，微博被转发的数目，youtube的视频的rating等。
5. Presence：即一个用户是否将自己的位置和availability显示给其他user看，即这个user是不是把真正的自己publish出来，网络到现实。
6. Conversation：表示了media的传播，通常之内容和传播速度。
7. Sharing：即分享信息给其他的user

这个theory表示了user在使用social media的基本全部动机，而不同的social media platform，强调的是不同特点，比如youtube就强调share，而facebook就强调Relationship。


### Cognitive
社会人认知强调的是cognition process在社会情景中的变化和结构。暗示环境，人的行为，和认知是互为因果的。而人的cognitive通常是三个level：
1. Cognition：通常指个体的感觉，比如读，写，记忆的信息，强调信息本身
2. Metacognition：即自己对自己cognition的一种认知，例如我自己控制我的感受和信息，监视自己的情绪，控制自己的行为。
3. Epistemic Cognition：表示knowledge related问题，例如knowledge limitation，确定知道，这种。

例如，给出26只绵羊和10只山羊，那么牧羊人多少岁了？这个问题很多人无法回答，因为Epistemic cognition层面上，这个问题触及到了knowledge limitation，但是很多小孩子却会尝试去猜这个问题的答案。Social Epistemic Cognition(SEC)可以引起collaborative knowledge construction，例如我们通过问卷的方式去交叉一些社会观念，或者学生通过在线讨论互相学习。


# Lecture 5 Sentiment Analysis & Opinion Mining

### User-Generated Content
通常在社交媒体中，user产生的信息有下面两种：
1. Fact: 表示事实，例如 “明天大围有艺术展” 这种信息。
2. Opinion & Feeling：表示user更加主管的感受，例如“我觉得这个电影很有趣”。

### Emotion
对于每一个人emotion，自我通常包括以下几点：
1. Cognitive：认知，包括知识，批判性思维
2. Emotional：感情，通常是感受或者社会关系
3. Physical： 物理层面，包括动作和五官的感觉。

感情(emotion)是会因为环境和被激发的，并且这是我们人类对于生存进化而来的。同时感情是共通的，英文中有550个词用于描述感情，但是人和人之间不需要任何词汇就可以做到感情上的交流。并且有研究表明，脸的下半部分表达的感情比上半部分要丰富...

Nonverbal communication：通常代指不需要语言的沟通，例如手势，面部表情，音调，眼神等等。













