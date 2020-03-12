---
layout: post

title: "ThoughtWorks中的敏捷团队建设"
date: 2016-10-22
categories: [AGILE]
tags: [Agile]

author: "袁慎建"

brief: "主导项目成功交付的核心因素还是人，<i>以人为本</i> 在软件交付项目团队中也是非常适用的。在ThoughtWorks，唯一相同的是每个人都有独自的个性和特长，那么这些有个性的小伙伴们是如何良好地在一个团队中工作的呢，除了大家比较强的自我管理能力之外（对TW的价值文化的认同，自我管理和自我驱动力较强），也需要有意识的搞一些团队建设工作，特别是Team Leader(PM, TL)要兼顾团队建设方面的职责，思考如何让团队更快地成长起来，以及营造更融洽的团队氛围。"


---


* content
{:toc}

---

>本文以我在[ThoughtWorks](https://thoughtworks.com/)中的E项目经历为依据，介绍敏捷团队中所团队文化建设。
>
>想要学习了解[ThoughtWorks](https://thoughtworks.com/)中的敏捷实践，不要错过 [我在ThoughtWorks中的敏捷实践]({{'/first-impressive-agile-experience-in-thoughtworks/'}}) 这篇文章。

## 项目回顾
如果你已经阅读过 [我在ThoughtWorks中的敏捷实践]({{'/first-impressive-agile-experience-in-thoughtworks/'}})，可以跳过`项目回顾`，直接进入 [团队建设](#section-3)。

---

### 项目背景
E项目是一个在线的物资跟踪监控系统。由[ThoughtWorks](https://thoughtworks.com/)团队为客户提供的一套完善的软件交付服务。

该系统为资助物资的跟踪与监控提供了完整的网络解决方案。整个流程涵盖了客户对物资来源的管控、库存管理、物资下发与跟踪、客户与IP(Implementing Partner，合作伙伴)的协作沟通、IP对运输结果的反馈(生成报告，警告通知，短信互动，邮件通知)，以及IP对物资的二次分发后的记录跟踪与监控。

该项目属于海外独立交付项目，整个开发过程由[ThoughtWorks](https://thoughtworks.com/)团队负责管理。好了，项目信息只能透漏这么多了，不然客户要找我打官司了。

---

### 成员背景
[ThoughtWorks](https://thoughtworks.com/)提供完整的交付团队(`PM`, `BA`, `TL`, `QA`, `DEV`, `UX`)，团队为颇具代表性的敏捷团队，规模10人左右。客户团队主要接口人3位。

[ThoughtWorks](https://thoughtworks.com/)团队成员，犹如一架生猛的战斗机：PM英文一流，敏捷开发管理相当到位，因为看了上万本`脑残`小说，时不时就用到了生活中来。TL拥有7年以上的开发经验，7年之痒，什么，不用说都懂的。TL还富有学习激情和被黑精神，黑历史墙列满了他的关辉`血泪史`。QA被誉为IT界的福尔摩斯，DEV都休想逃过她敏锐的鼻子（怎么是鼻子呢？太陶醉了吧，这是境界！），最后，就是`'苦逼'`的DEV，也就是以`程序员`自居的我们。

我们每个人特点各不相同，但有一点神似：`专业，责任心，追求卓越`

我们团队还有一个`workout`的文化特色，向⬇️看：

![Alt text]({{ site.url }}{{ site.img_path }}{{ '/agile/agile-development-summary-of-project-e-sport.jpg' | prepend: site.baseurl   }})

>PS：我们的`workout`自开始之日到项目交付之期，不曾落下过一天，且收到良好的反馈。即便团队成员分开了，每个人都能将`运动精神`传播下去，乃至源远流长......

---


## 团队建设
>主导项目成功交付的核心因素还是人，`以人为本`在软件交付项目团队中也是非常适用的。

[ThoughtWorks](https://thoughtworks.com/)中，唯一相同的是每个人都有独自的个性和特长，那么这些有个性的小伙伴们是如何良好地在一个团队中工作的呢，除了大家比较强的自我管理能力之外（对TW的价值文化的认同，自我管理和自我驱动力较强），也需要有意识的搞一些团队建设工作，特别是Team Leader(PM, TL)要兼顾团队建设方面的职责，思考如何让团队更快地成长起来，以及营造更融洽的团队氛围。关于团队建设，我翻译过两篇关于TL的文章：[高效技术领导的5个锦囊妙计]({{ site.url }}{{"/5-tips-for-being-an-effective-tech-lead/"}})，[初次做技术领导的三个陷阱]({{ site.url }}{{"/3-common-mistakes-of-first-time-tech-lead/"}})，欢迎阅读。

---

### Team Session
>学习的乐趣在于分享。一个了不起的团队需要营造出一种浓厚学习的氛围，每个人都在驱动自己去学习提升，努力将所学的知识分享出来，自己成长了，团队也在进步。


我们Team在第一次[Retro](#retro)中总结出来要执行Team Session，简单来讲就是某个人分享一个主题，将自己Get到的技能通过演讲的形式在团队中分享出来，让团队每一个人都能获益。分享的主题可以是技术和非技术的（技术占的比例大），主题与项目正在使用的或者即将尝试的技术栈相关，也可以是在开发过程中的一些实践总结。

我们最开始商量出来的方案是：每天中午15分钟自由讨论时间，每周周五中午1：30~2：30有一个主题Session，由不同的人去Take。项目交付之后，再回过头去看，是有一些可以提高的地方的，比如说每天中午15分钟这个很少聚在一起讨论（刚吃完饭，休息，看看新闻等），这个自由讨论其实可以放在[Code Review](code-review)中穿插进行。而每周的主题Session，一开始的时候有几次分享，中间很多时候是空着的（不同的原因），整个项目下来，我也只做了一次Session（时间2个多小时）。所以在后面我们做了一些总结，讨论出方案：

- 每周指派一个Session的Owner，Owner需要对这周的Session负责，自己本周至少有一场主题Session，也可以有多场（非常欢迎的事情），也可以邀请团队其他成员甚至团队外的人来讲Session。
- 如果Owner因为有其他的原因没有任何Session，需要团队其他成员同意。（实在有事，大家还是可以理解的，但是本着我们黑人到底的精神，至少会想出一些惩罚措施，比如说请吃冰棍。比如说请吃新辣道，额，广告一下，新辣道是一家火锅店）
- 鼓励在团队外做Session，比如公司级别以及社区级别的，将团队传播出去，让我们团队更具有影响力。这就需要更多的练习实践，而Team Session是一个好的练习机会。


经过改良后，效果很好，大家的参与度较高，目前一直沿用中。

>PS:在[ThoughtWorks](https://thoughtworks.com/)这种扁平化组织中，几乎没有什么东西是可以强制的，这个是我们Team共同讨论出来的方案，不具备强制性。大家达成一致：8个人，平均每人两周一次Session，不但可以较轻松愉快的完成，利己利人。

---

### One2One
>Team Leader组织定期一对一的交流，能够更好地把握团队成员的心理状态和成长预期，从而更好地把控团队的成长方向及达成交付目标。

One2One，通常是PM定期（一个月）跟团队成员的一个单独面对面交流的机会。时间一般控制在半个小时，主要了解每个人对目前团队的看法，提一些自己觉得做得好的以及不好的点，聊一聊自己在这段时间工作的状态，以及每个人目前在团队中的成长是否符合自己预期。

我在One2One Meeting的时候，除了上述内容，我会加一个索要反馈的环节。了解一下PM视角中的我以及团队的状况，了解一些PM视角中其他成员的优势，以及给自己的一些成长建议。


---

### Workout
>身体是革命的本钱。老生常谈了，不说也懂的。

我在加入[ThoughtWorks](https://thoughtworks.com/)之前就从一些途径了解到TWers在日常工作中有[平板支撑](http://www.tabatatimer.com/)环节，也就是本文刚开始的时候就提到的运动文化。2015.03.26加入[ThoughtWorks](https://thoughtworks.com/)以后，发现并不是每一个Team都会集体做运动，而我是一个非常喜欢运动的人，也从运动中获益不少。所以，我不管走到哪个团队，我都会组织大家一起做平板支撑，让我感到意外的是，每个人的参与度很高，这也一直在鼓励我在Team中将运动进行到底。

运动会耽误工作时间吗？这是一个值得讨论的话题。简单算一笔账，30s * 10r + 10s * 10r + 10s = 410s，一共不到7分钟。七分钟，并不会耽误工作的时间，那么它带来的好处呢，从大家的反馈中说明一个事实：`腰椎和颈椎得到了放松`、`工作效率提高了`、`团队气氛变好了`...

关于运动，有一些简单的实践:

- 运动主要是活跃团队气氛，让大家忙里偷闲一会儿，同时让身体得到锻炼放松。切不可顽固不变通，大家开心的参与到其中，中间聊一些幽默风趣的话题（这个跟团队成员有关，比如说我们Team几乎每个人都很幽默，尤其是“脑残”的PM和TL）。
- 每天在一个固定的时间开始运动，鼓励每个人都参与，但不强制。
- 运动的强度，取决于多数人的反馈，不可强度太大，也不必太轻松。
- 欢迎其他团队的成员加入，鼓励他们去组织自己团队做运动。-e-
- 团队众筹购买瑜伽垫，垫着会让胳膊肘子更舒服一些。
- 对于平板支撑，胳膊肘子成90度，支撑的过程不要让肘子有蠕动（皮肤细嫩的女生会被磨破皮），收腹，保持臀部腰部成一条直线，切记不可让腹部吊着，这样腰部受力对腰不好。
- 平板支撑的支撑时间不是越长越好，间隔着休息，建议支撑时间20~30秒，休息时间10秒，6~10组。

下面是我们Team的Workout剪影：

![Alt text]({{ site.url }}{{ site.img_path }}{{ '/agile/agile-development-summary-of-project-e-workout.jpg' | prepend: site.baseurl   }})

___


### Team Building
>抛开工作聚在一起，放松的玩耍聊天吃饭，是为了保持更好的工作状态和团队氛围。工作也是生活中的一部分，不必让生活一直处于紧张状态。

Team Building，可以理解为团队建设，在实践中最常见的实践是大家一起聚个餐，玩个密室逃脱什么。在我司，每个人工每月都可以使用一定额度的经费，进行团队聚餐娱乐，也就是做一些工作之外建设团队的事情。这么做主要是让团队能够以放松的状态聚在一起，除了工作，天南地北，幽默逗乐，样样都来，酒饱饭足之后（基本上不喝酒），重新高效的投入到工作中。很多国内的公司也有聚餐这回事，他们通常是庆祝什么活动和功勋，而我们是为了建设团队。

Team building每月一次，以轻松为主，有吃饭环节，也有挑战冒险的娱乐活动，根据团队的意愿不一样。TB的重点是团队成员聚在一起做一些有趣的事情，培养团队成员的感情和凝聚力。比如说，我们有一次去玩密室逃脱，整个Team都在绞尽脑汁破解模式的关卡，知道胜利走出密室那一刻，我们的情感也会增加几分。形式各异，只要不太占用工作时间，一般是半个工作日以内，也可以选择节假日，只要达成一致即可。

>这个是因为我司文化的特殊性，鼓励这种活动，并给与经费支持。不同的公司，团队也可以采取这种方式去把大家聚在一起，可以是领导请客（一般可以报销），可以是Team AA。


下面是我们Team building的图集：

![Alt text]({{ site.url }}{{ site.img_path }}{{ '/agile/agile-development-summary-of-project-e-tb.jpg' | prepend: site.baseurl   }})

___


## 总结
团队建设的核心是增强团队的战斗力（技能）和凝聚力（氛围）。工作上不必太死板苛刻，学会融通，多一些包容，尽量让每一个人特长都能发挥出来，让每个人感觉自己的存在是能为团队创造价值的，并且自身能力也能得到成长。也不要遗忘对其他人发出赞美与肯定，还有生活上的关怀。

当然，如果团队中有几个快乐的逗比那是得天独厚的资源，而我们Team中几乎都是逗比，也很快乐。

>PS
1. 本文截图涉及到多个TWers的照片（其中有大牛[熊杰](http://gigix.thoughtworkers.org/)），都经由同事的许可，遂敢于毫无顾忌的发表。
2. 文章中提供的链接在有网络的情况下如果不能访问，确认自己是否可以翻墙，如不可以，切勿较真。

---

## 术语注释
* PM：Project Manager，项目经理
* BA：Business Analyst，业务分析师
* TL：Technical Leader，技术领导
* QA：Quality Assurance，测试人员
* DEV：Developer，开发人员
* UX：User Experience，用户体检设计师
* AC：Acceptance Criteria，验收条件
* UAT：User Acceptance Test，用户验收测试
* Retro：Retrospective，回顾会议
* TB: Team building，团队建设



