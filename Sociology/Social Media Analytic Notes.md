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


# Lecture 2

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








