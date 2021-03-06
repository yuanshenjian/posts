---
layout: post
title: "构建可持续部署的Pipeline"
date: 2017-01-31
categories: [CI]
tags: [Continuous Integration]
author: "袁慎建"

brief: "CI的核心目标是快速频繁集成代码，通过一些手段（编译、代码检查、运行测试、覆盖率等）来持续地提供及时有效的反馈，可持续从何而来？前提就是这些手段都是行之有效的。</br></br>
将CI的那些手段对应到每一次集成中的各个步骤，这些步骤应该是值得信赖的，比如单元测试、集成测试、E2E测试，确保它们都能够真实有效地反馈当前代码库的集成状态。试想一下，集成中单元测试步骤虽然存在，但其实没有运行任何有用的测试，亦或代码中没有添加任何测试，那么即便这一步通过了，发布的软件也是不可靠的。持续之根本取决于每一步的可靠性"

---

* content
{:toc}

---

## 持续之根本

>CI的核心目标是快速频繁集成代码，通过一些手段（编译、代码检查、运行测试、覆盖率等）来持续地提供及时有效的反馈，可持续从何而来？前提就是这些手段都是行之有效的。

将CI的那些手段对应到每一次集成中的各个步骤，这些步骤应该是值得信赖的，比如单元测试、集成测试、E2E测试，确保它们都能够真实有效地反馈当前代码库的集成状态。试想一下，集成中单元测试步骤虽然存在，但其实没有运行任何有用的测试，亦或代码中没有添加任何测试，那么即便这一步通过了，发布的软件也是不可靠的。持续之根本取决于每一步的可靠性。

以一个Java Web工程为例，一次集成通常含有以下步骤：

![]({{ site.url }}{{ site.img_path }}{{ '/dojo/ci/ci-steps.png' }})

这里面的每一个步骤循序渐进，必须都是通过后才能持续往后走，而通过也必须是有意义的通过，而不只是亮了一个空壳绿灯。

>实践指导：
>有些项目的E2E测试运行时间很长，E2E测试可能就选在夜间运行，而不是在打包前必须要运行的步骤。另外，有些项目只有单元测试。

---

## CI何所为
>所谓CI，本质上在一个可被触及的中心服务器去集成那些提交到中心代码库的代码，并将一些原本可以重复手工完成检验步骤自动化起来。

无CI论者`NoCI`会说：“CI其实没啥用，你们大可把代码提交，我pull下来做集成。我在我的机器跑测试，然后进行部署，一旦挂了我通知你们...”

一句话包含了多少辛酸和无奈：

```
1. 每天那么多次提交，NoCI这哥们完全不能知晓其他人何时提交。
2. 运行了测试挂了，通知谁？都不知道是谁的提交导致挂了，提供不了即时的反馈。
3. 即便别人提交后通知他，那他每天要多次重复去做这项工作，恐怕没有什么产出了，就等着被fire了。
4. 自己机器还要开发，还要运行很多其他程序，运行测试跑了1个小时，一天跑八次测试就该下班了。
```

听起来，`NoCI`这哥们也能做这些事情，只不过效果没那么好，还附带了被Fire的风险。实际上，CI也并不神奇，一句话`简单`概括CI何所为:

```
CI做的事情就是将重复的手工工作自动化管理起来，并提供即时有效的反馈。
```

>实践指导：
>反过来想，CI能做的事情，你都能在本地手动去做，所以在搭建CI的时候，可以现在本地手动验证你在CI所设置的任务是否正确。

---

## 踏上征途
从毕业到现在，经历了多个不同项目，从`!测试 && !CI`-->`本地测试`-->`CI && 测试`-->`强制 CI && 测试`，对CI历经了从0到1的过程，一开始非常享受CI所带来的好处，如今便是`身处酒巷，久而不觉酒香`的状态：在一个项目启动的时候，首先会在Iteration 0将CI作为必备的基础设施搭建好，然后步入开发阶段。

CI策略由简入繁，简单的比如一个Android的build，跑完单元测试、打包apk、发布apk就完事了，复杂的就好比一个微服务的CI，每一个微服务除了完善的`单元测试`以及`严格的集成测试`，还需要定义清楚各个微服务之间如下一些构建依赖关系：

```
1. 构建的顺序，服务B会依赖服务A的构建参数或者只有A成功后才构建B。
2. 微服务部署的顺序，定义哪些服务部署是允许失败的，以及失败后的救援措施。
3. 微服务在多个环境的部署时的版本控制。
```

实际中的CI实践通常介于简单和复杂之间，这里我以过去几个项目经历为常规CI的依据，主要包含了四个步骤：

```
1. 解析依赖（编译）。
2. 运行测试。
3. 打包。
4. 部署/发布
```

最重要的属2和4。因为测试是代码库质量的保证，而测试也是我们搭建CI的前提，没有测试的CI犹如牛刀杀鸡。而部署则是关系到软件的交付，只有做到自动化部署发布，CI才算完成了它的最后使命（那些由某些客观因素导致无法自动化部署的场景不在讨论范围之内）。

---

### 解析依赖（编译）
CI通常开始源代码的获取，获取了所有完整的代码库后，首先要做的事情便是解析项目所需的依赖并经行编译。为什么会解析依赖呢？在实际开发过程中，项目往往依赖很多第三方类库，开发人员在搭建本地开发环境的时候，首次需要消耗一定的时间去下载依赖，在后续开发过程中，只要所下载的依赖没有被删除，就不会重复下载。而CI也少不了这个过程，所以第一次构建的时候，也会需要去下载这些依赖，然后进行编译。所以需要保证`CI服务器能够正常下载第三方依赖`。

当然，也有人说，把这些依赖直接放入代码库中不就完事了吗？对，这就是我正要提的一个反模式，为什么称之为反模式呢，以下简单列出这种实践的几个弊端：

```
1. 代码库体积过大。第三方依赖往往体积较大（多则几个GB），提交到代码库会导致代码库体积很大，而我们应该有责任去让代码库尽量保持精简。
2. 影响部署效率。第三方依赖体积过大，导致每次部署的时候需要传输的大量文件，较大程度影响了部署流程，降低效率。
3. 版本冲突。团队中开发人员A升级了依赖的版本，而其他开发人员在本地也进行了升级，提交后会难免会引发冲突，造成不必要的麻烦。
4. 违背了DRY(Don't Repeat Yourself)。因为每个人都保存了一份完整的依赖，一旦有改动，需要通知并传达给所有人大量的信息，而如果每个人只是持有依赖的定义（配置文件），相当于持有对象的引用，所有人根据引用来更新依赖，他们之间需要传递的信息就变得非常精简。
```

>实践指导：
>当开发、CI只能在内网中进行时，就无法从互联网上下载依赖，此时需要在CI所在的内网中搭建一个私有仓库，用来专门存放第三方依赖，项目所需要的依赖从该仓库中下载。比如Java工程，可以搭建一个私有的Maven仓库，Gradle在解析依赖的时候，将源指向该私有仓库。

---

### 测试
运行测试的策略通常借鉴于[测试金字塔](https://martinfowler.com/bliki/TestPyramid.html)，单元测试、集成测试、E2E测试。关于测试的最佳实践则是将这三部分测试都涵盖进去。首先CI会每次自动运行单元测试，然后自动运行集成测试，最后是E2E测试。完美的情况下，每一次提交三部分测试都会运行，只有所有测试通过后才进入下一个环节。而在实际中，有些项目E2E测试运行的时间较长，以至于对集成和部署造成了一定的影响，此时我们需要做一些优化措施，可以并发运行E2E测试(Jenkins中join pulgin)：

![]({{ site.url }}{{ site.img_path }}{{ '/dojo/ci/e2e-test-slaves.png' }})


>实践指导：
>测试三步曲中，最难的是数据准备，要做到测试能够并发运行，就需要在设计E2E测试用例的时候，保证Test Case的准备数据和数据回滚的独立性，但考虑到每一个Test Case都完全独立，成本较大（数据库数据的初始化和回滚），通常是对同一个功能模块中存在依赖关系的Test Case做一次数据准备和数据回滚，Test Case则按照一定的顺序运行。或者根据并发Salve的粒度来划分独立边界。

关于测试数据准备和数据回滚的边界，举个例子：

```
一个电子商务系统中有登录、用户管理、地址管理、商品管理模块、购物车5个模块，我们将登录、用户管理和地址管理模块的测试放在的Slave A上，将商品管理和购物车模块的测试放在Slave B上运行，边界划分的粒度就存在以下三种：
1. 以单个Test Case作为边界，运行任何Test Case都会做一次初始化和回滚。（非常耗时）
2. 以功能模块作为边界，每运行一个模块的测试做一次初始化和回滚。（较耗时）
3. 以Slave作为边界，比如Salve A在运行E2E测试的只做一次初始化和回滚，且Slave A和Slave B的数据互不干扰。（推荐的方式）
```

---

### 打包
>之前某位UX跟我们开发人员要炸包，一开始把我愣住了，后来得知她原来是想问工程构建的jar包。

没错，Java开发人员最常接触包就是jar文件（jar包）或war文件（war包）。另外，Android工程则会输出apk，IOS工程会输出ipa，JavaScript工程则会输出丑化后的css和js文件，等等。所以，CI打包步骤是继测试后的一个打包步骤，它为部署做准备。以Java Web工程为例，CI在测试通过后，构建出一个war包，之后就将该war包存档起来，供下一步使用。所构建的war需要兼顾以下几点：

```
1. 包含程序运行所需要的第三方依赖。
2. 包含项目中涉及的资源文件（css、js、image、icon等）。
3. 压缩css和js相关资源。
4. 不包含任何测试相关的代码和类库。
5. 不包含任何程序运行时不需要的文件。
```

### 部署/发布
通产情况下，部署/发布 往往被认为是从开发到生产环境的最后一步，在本文的范畴中，我也把它列为了最后一步（实际中，部署到生产环境后，运营监控才刚刚开始）。

依赖解析完成、自动化测试都通过、软件包构建完毕，此时我们需要将软件包部署到生产环境中供测试和用户使用了。有人会说：“这个简单，直接将软件包部署到生产环境的服务器上，启动服务不就完毕了！” 真有这么简单？先来尝试回答一下几个问题：

```
1. 直接部署到生产环境，软件不存在漏洞了吗？
2. 软件在交付前，回归测试能够直接在生产环境上进行吗？
3. 在敏捷开发中，需要定期给客户showcase，此时用什么环境来demo呢？
4. 软件在正式上线前，用户通常会安排一个期限的UAT（用户验收测试），这个环境又如何来？
5. 重新部署到生产环境，如果用户正在使用着软件怎么办？
```

以上几个问题引出了经典的`Test`、`Staging`、`UAT`、`Production`四个环境。分别用于测试、演示、用户验收、生产运营。另外，开发人员通常在本地机器会运行一个`Dev`环境。下图总结了以上四个环境的部署方式：

![]({{ site.url }}{{ site.img_path }}{{ '/dojo/ci/deployment-envs.png' }})

`UAT` 和 `Production` 环境被虚线框起来，因为这两个环境在某些情况下是交付团队是没有权限去控制的，此时是由客户的专门人员负责一键部署。

>实践指导：
>通常部署后需要管理多个服务的启动，而且这些服务存在依赖关系，它们的启动顺序也是有一定先后，可以使用[surpervisord](http://www.supervisord.org/)，来管理这些服务的启动顺序和重启策略。

---

## 总结
对于一个敏捷团队（高效协作和快速反馈），CI是项目交付中必不可少的基础设施，无论你是一名开发人员还是测试人员或者是Team Lead（PM），都应该尽全力捍卫CI的地位。

---

## 注释
* CI： Contiuous Integration，持续集成
* PM：Project Manager，项目经理
* UAT：User Acceptance Test，用户验收测试








