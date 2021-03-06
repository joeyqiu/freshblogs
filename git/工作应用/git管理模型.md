## git 管理模型

纯粹参照git的源代码管理模型－－[git flow](http://www.ituring.com.cn/article/56870)



![git flow流程图](./assets/gitflow.png)


分为**主分支**和**辅助分支**两类。其中主分支用于组织与软件开发、部署相关的活动；辅助分支为了解决特定的问题而进行的各种开发活动。


### 主分支
主分支是所有开发活动的核心分支。所有的开发活动产生的输出物最终都会反映到主分支的代码中。主分支分为**master**分支和__development__分支。

#### master分支
master分支上存放的应该是随时可供在生产环境中部署的代码(Product Ready state)。当开发活动告一段落，产生了一份新的可供部署的代码时，master分支上的代码会被更新。同时，每一次更新，最好添加对应的版本号标签(TAG)。

#### develop分支
develop分支是保存当前最新开发成果的分支。通常这个分支上的代码也是可以进行每日夜间发布的代码（Nightly build）。因此这个分支有时也可以被称作"integration branch",

当develop分支上的代码已实现了软件需求说明书中的所有功能，通过了所有的测试后，并且代码已经足够稳定时，就可以将所有的开发成果合并回master分支了。对于master分支上新提交的代码，建议都打上一个新的版本号标签（TAG），供后续代码跟踪使用。

因此，每次将develop分支上的代码合并回master分支时，我们都可以认为一个新的可供在生产环境中部署的版本产生了。

通常而言，**仅在发布新的可供部署的代码时，才更新master分支上的代码**时推荐所有人都遵守的行为标准。


### 辅助分支

辅助分支是用于组织解决特定问题的各种软件开发活动的分支。辅助分支主要用于组织软件新功能的并行开发、简化新功能开发代码的跟踪、辅助完成版本发布工作以及对生产代码的缺陷进行紧急修复工作。这些分支和主分支不同，通常只会在有限的时间范围内存在。


辅助分支包括：

* 用于开发新功能时所使用的feature分支
* 用于辅助版本发布的release分支
* 用于修正生产代码中缺陷的hotfix分支


#### feature分支
使用规范：

* 可以从develop分支发起feature分支
* 代码必须合并回develop分支
* feature分支的命名可以使用`master`, `develop`, `release-*`, `hotfix-*`之外的任何名称

feature分支（有时也可以被叫做**topic**分支）通常是在开发某一项新的软件功能的时候使用，这个分支上的代码变更最终合并回develop分支或者干脆被抛弃掉（例如实验性且效果不好的代码变更）。

一般而言，feature分支可以保存在开发者自己的代码库中而不强制提交到主代码库里。


#### release分支
使用规范：

* 可以从develop分支派生
* 必须合并回develop分支和master分支
* 分支命名惯例: `release-*`

release分支是为发布新产品而设计的。在这分支上的代码允许做小的缺陷修正、准备发布版本所需的各项说明信息(版本号、发布时间、编译时间等等)。通过在release分支上进行这些工作可以让develop分支空闲出来以接受新的feature分支上的代码提交，进入新的软件开发迭代周期。

当develop分支上的代码已经包含了所有即将发布的版本中所计划包含的软件功能，并且已经通过所有测试时，我们就可以考虑准备创建release分支了。而所有在当前即将发布的版本之外的业务需求一定要确保不能混到release分支之内（避免由此引入一些不可控的系统缺陷）。

#### hotfix分支
使用规范：

* 可以从master分支派生
* 必须合并回master分支和develop分支
* 分支命名惯例： hotfix-*

除了是计划外创建的以外，hotfix分支和release分支十分相似：都可以产生一个新的可供在生产环境部署的软件版本。

当生产环境中的软件遇到了异常情况或者发现了严重到必须立即修复的软件缺陷的时候，就需要从master分支上置顶的TAG版本派生hotfix分支来组织代码的紧急修复工作。

这样做的显而易见的好处是不会打断正在进行的develop分支的开发工作，能让团队中负责新功能开发的人与负责代码紧急修复的人并行的开展工作。





## 工作上的git管理

目前的情况是多人协作开发，提测环境只有能用一个分支dev，或者master，所以对于这种情况，git的管理情况如下：

主分支：master, develop

发开的时候基于master切一个feature分支，然后开发的时候，需要提测的时候把代码merge到dev分支上去，如果测试ok，则把代码merge到master分支上去。



之所以不把dev和master分支互相合并，是因为这边dev上，有很多提测代码，避免带到master分支上。