---
layout: post

title: "微服务架构下的测试应对策略（上）"
date: 2017-07-05
categories: [eXtreme Programming]
tags: [eXtreme Programming, TDD]


author: "袁慎建"

brief: "
微服务架构解决了单体应用的痛点，打破了SOA的瓶颈，同时也带来了很多的复杂性。部署运维方面，服务的部署、管理、监控。开发设计方面，服务的拆分、设计、编码、测试都将会变得复杂。幸运的是，容器化技术（比如无比流行的Docker）已经很大程度上帮助我们克服了环境的差异性，而一些容器编排工具诸如Kubernetes, Rancher, Docker-compose 提供了容器部署管理的解决方案。作为行业的领航者，ThoughtWorks也在极力倡导 开发、设计、部署、运维一体化 的DEVOPS文化理念，并通过丰富的咨询和交付成果来帮助企业研发团队更好地实施微服务架构的开发。
<br/><br/>

那么在编码测试方面，又有什么新招来保证微服务架构下系统的质量？

<br/><br/>

本文将从开发测试的视角来探讨如何在微服务架构下通过不一样的测试策略来尽可能的保证系统的质量。
"

---

* content
{:toc}

---

## 系统架构的演变
伴随着互联网的快速发展，Web应用系统从面向企业内部发展到面向市场用户，业务的日趋复杂以及用户量的上升，那些曾经工作良好的单体应用开始遇到开发、测试、部署、发布各个方面的瓶颈，诸如`扩展新增功能艰难`、`系统庞大难以维护`、`编译太耗时，发布流程太慢`等问题困扰着开发团队。

SOA的问世促使系统架构发生了跨越式的演变，它提出了面向服务的架构思想，将系统拆分成多个服务组件，并通过ESB（企业服务总线）对服务组件进行统一管理，但重量级的ESB使得自身又成为了一个瓶颈。随之而来的是近来业界流行的微服务架构，它将SOA的思想进一步升级，将系统组件化、服务化以及去中心化，强调 `轻量级`、`松耦合`、`服务自治`、`独立部署`。

微服务架构解决了单体应用的痛点，打破了SOA的瓶颈，同时也带来了很多的复杂性。部署运维方面，服务的部署、管理、监控。开发设计方面，服务的拆分、设计、编码、测试都将会变得复杂。幸运的是，容器化技术（比如无比流行的Docker）已经很大程度上帮助我们克服了环境的差异性，而一些容器编排工具诸如Kubernetes, Rancher, Docker-compose 提供了容器部署管理的解决方案。作为行业的领航者，ThoughtWorks也在极力倡导 *开发、设计、部署、运维一体化* 的DEVOPS文化理念，并通过丰富的咨询和交付成果来帮助企业研发团队更好地实施微服务架构的开发。

*那么在编码测试方面，又有什么招来保证微服务架构下系统的质量？*

本文将从开发测试的视角来探讨如何在微服务架构下通过不一样的测试策略来尽可能的保证系统的质量。

---

## 单体应用测试实践

>当我们的意识中只存在一样东西的时候，我们便可以不假思索的拿来就用。

在单体时代，对于开发-测试-部署，业界已经具备了一套很成熟的解决方案。基于这种方案，当一个敏捷开发的小Team开始构建一个应用之前，CI搭建的过程也会变得非常简单：*CI只需要从一个代码库中去pull代码，然后编译-测试-部署*，它的流程可以简化成：


在这种单线流水线模式下，如果团队的自动化实践做得很好，开发人员只需要关注自己编写代码时所编写的测试的质量和数量。整个应用的测试策略简单直接:

![]({{ site.url }}{{ site.img_path }}{{ '/xp/single-app-ci.jpg' }})

***保证足够的单元测试的覆盖率，保持一定数量的Servcie测试，添加一些重要业务流程的E2E测试。***


---


## 微服务测试的演变
微服务架构是一种演进式架构，开发团队跟领域专家在一起进行业务分析（Event Storming），从而划分出独立的服务，系统一开始确定为独立服务的数量可能是几个，伴随着业务的复杂深入，会不断地衍生出新的服务。下图是一个包含了四个服务的微服务架构的系统：

![]({{ site.url }}{{ site.img_path }}{{ '/xp/microservice-architecture.jpg' }})

微服务体系中的诸多服务不可避免跨服务调用，它们通常使用轻量级的HTTP RESTful API。那么如何保证跨服务调用的可靠性以及整个系统集成的质量？尤其是当不同服务由不同小团队负责开发和测试。

---

### 服务自身的Unit测试
系统被拆分成独立的服务，每个服务都是一个完整的小系统，首要工作仍然是保证服务自身的业务功能的正确性。比如一个Java Web应用（Springboot），API功能以及各个Service的业务逻辑的正确性，可以通过单元测试来保证。服务细分之后从某种意义上让单元测试更加易于编写，可以借助测试替身来屏蔽掉对其他服务依赖。

---

### 系统级的集成（UI）测试
Unit测试使得开发人员可以快活地活在自己的世界中，每个开发团队按照图纸造出系统的一个部件，只有当这些小部件集成在一起之后能够按照用户的期望为用户提供服务才体现出了系统业务价值。所以我们要通过系统集成测试（UI测试）来保证集成的质量。


从 [测试金字塔](https://martinfowler.com/bliki/TestPyramid.html) 中可以看出，在一个系统中，UI测试是数量最少的。虽然它的业务价值最高，但它高昂的成本使得它只会覆盖业务流程复杂的业务场景。甚至，当一个微服务架构系统中服务个数量达到一定之后，很多开发团队对UI测试开始望而却步，因为在一个存在多个服务的系统中（即便单体应用系统）做集成测试，会面临诸多痛点：

- 需要维护完整的运行环境，成本很高。
- 环境不稳定（UI不稳定）导致测试随机挂，功能增强很容易破坏大量测试。
- 问题难定位，修复时间太长，影响Pipeline的推进。
- 运行速度慢，反馈周期长。
- 存在重复测试已测试的功能。

这些痛点在很大程度上会削减一个开发团队的生产力，某些企业会雇一个QA进行重复的人工测试从而解放开发人员的生产力。这种措施有悖于追求卓越的理念，并没有从本质上解决系统的集成的质量问题。既然UI测试已经不适用引进了微服务架构的开发团队，要如何保证服务集成的质量，我们还需要在自动化测试道路上另辟蹊径。

---

### Pair集成测试
系统级别的集成测试阻碍重重，我们不妨退一步思考，将集成的范围缩小 *保证服务俩俩的集成的可靠性*。有了这个想法，我们开始对服务俩俩配对做集成测试。测试架构演变成：

![]({{ site.url }}{{ site.img_path }}{{ '/xp/pair-integration-test.jpg' }})

我们需要真实运行待测试的服务，并且对其他服务使用替身。不难看出这种方式存在以下问题：

- 需要运行待集成的真实服务，存在环境不稳定导致维护成本增加。
- 需要Mock掉其他服务，增加了额外的工作量。
- 存在大量重复测试已经测试的功能。

虽然Pair集成测试没有从根本上解决UI测试的痛点，但它提出了 *积小成多* 的理念，该理念告诉我们：***只要能够保证服务俩俩之间的集成是可靠的，我们就可以相信系统集成也是可靠的。***

---

### 引入Contract概念的集成测试

就在两年前，我在珠海出差的某项目上跟小伙伴一起尝试了一种集成测试方案。当时项目采用的是前后端分离开发，后端作为服务提供者提供RESTful API，前端作为消费者消费API。

为了保证前后端开发人员并行开展工作，我们引入了Contarct概念。前后端开发人员基于业务共同定义API协议（Contract），该协议以JSON文件存在于代码库的测试资源目录中，前端在开发过程中以JSON文件作为测试的断言依据。而后端开发人员则参照该协议内容来实现API。

基于这种方案，前后端开发人员如果都遵守了协议，联调的过程就会非常顺利。而它的优势也很明显的体现出来：

- 不需要运行其他服务，环境简单，运行快。
- 测试可控范围缩小到单个服务内部。
- 按照Contract，各自编写代码并测试。

前后端本质上等价于服务提供方和服务消费方，所以该理念运用在微服务之间的集成测试中，系统的测试架构会得到进一步演进：

![]({{ site.url }}{{ site.img_path }}{{ '/xp/contract-concept-test.jpg' }})

我么在享受着它带来的好处的同时，问题也偷偷地潜入系统中。不久后，CI就报警了：*UI测试测试挂了*

进行一番debug之后我们定位到了问题，解开了 `按照Contract单独运行测试一切OK，为什么上集成环境就莫名其妙挂掉！` 的疑惑：

```groovy
// 两天前
request {
    method 'POST'
    url '/users'
    body([
            name: $(regex('[a-z]{6, 20}')),
            email: 'sjyuan@thoughtworks.com',
            homePage: 'http://sjyuan.club'
    ])
    headers {
        contentType('application/json')
    }
}

// 两天后
request {
    method 'POST'
    url '/users'
    body([
            name: $(regex('[a-z]{6, 20}')),
            email: 'sjyuan@thoughtworks.com',
            homePage: 'http://sjyuan.club',
            gender: 'M'
    ])
    headers {
        contentType('application/json')
    }
}

```
通过Git历史记录发现服务消费方（前端）将API协议更新了，而服务提供方（后端）没有同步修改实现。

回顾一下`引入Contract概念的集成测试`，之所以会出现协议的修改直到集成环境中才暴露出来，是因为缺乏自动化监控机制来提前发现问题并预警。让我们做进一步深入思考：*把同一份API契约作为服务提供方和服务消费方的测试断言依据，一旦契约被一方改动，则另一方的测试便会失败*。

归根结底，我们缺乏一种有效的强制约束来约束双方，[下一篇]({{ '/test-strategy-to-meet-microservice-architecture-2/' }}) 我将分享的`消费者驱动契约测试`可以提供这种约束。

