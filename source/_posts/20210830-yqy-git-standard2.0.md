---
title: 药企云GIT使用规范 v2.0
date: 2021-08-30 17:42:43
cover: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223163750.png
top_img: https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20220223163750.png
categories:
  - Git
tags:
  - git
  - gitkraken
---

# 药企云GIT使用规范 v2.0
> Git usage standards version 2.0

# 工具介绍-GitKraken
GitKraken是基于Git代码管理的一个UI管理器，拥有非常精美的界面，可以配合Gitlab、Github、Gitee来使用。

<img src="https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210830174243.png" style="width: 100%;margin: 0 auto" />

## GitKraken创建分支流程

> 根据TAPD的迭代进行创建

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909095251.png)

> 在master上创建分支（创建分支由组长创建）

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909095530.png)

> 正常迭代创建（dev/对应的版本号+qa环境）例：dev/vCOA-qa4

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909135904.png)

> 紧急迭代创建（feature/对应的版本号+qa环境+紧急需求ID号）例：feature/vCOA-qa4-1004332

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909100459.png)

> 紧急需求ID号

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909100655.png)

> 修复分支创建（hotfix/对应的功能名称）例：hotfix/order

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909101446.png)

## GitKraken合并分支流程

> 根据目前的工作流程，dev，feature，hotfix，这三个版块的分支，会进行合并（合并流程一致）

第一步、将当前开发分支合并到对应qa/xxx环境让测试进行测试（由开发者合并） 例：Merge dev/vCOA-qa4 into qa/qa4

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909102401.png)

第二步、将当前开发分支合并到release环境（由对应的模块负责人合并）例：Merge dev/vCOA-qa4 into release

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909135312.png)

第三步、将当前开发分支合并到master（由上线的负责人合并）例：Merge dev/vCOA-qa4 into master

![](https://cdn.jsdelivr.net/gh/hehuixiong/image-blog/20210909135406.png)

## GitKraken注意事项
> 开发分支不能合并开发分支，比如：dev/xxx不能与feature/xxx合并
  qa环境不能合并其他分支，release环境不能合并其他分支.
  所有的分支创建都是由master拉取.
  遇到代码冲突的问题谨慎处理，避免造成代码丢失的问题.
  如果没有把握处理请求对应的开发一起看或者找组长.