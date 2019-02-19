---
title: Agile Software Development
layout: post
date: 2018-03-29 22:46:22
comments: true
tags: [agile]
categories: [agile]
keywords: agile develop
description: Agile Software Development
---

## Agile Meeting

![](/images/agile-software-development/agile-meeting.png)

#### Kick Off
  * 频率 ： 每张story
  * 时长 ： 3分钟
  * 内容 ： 在Dev领取Stroy前，需要Dev与BA、QA一起过Story的描述和AC，在三方都达成共识的情况下进行请下来的开发
  * 参会人员 ： BA、DEV、QA

<!-- more -->    

#### Stand Up
  * 频率 ： 每天早上
  * 时长 ： 10 - 15分钟（根据团队大小进行调节）
  * 内容
    * 工作在哪张卡上
    * 昨天做了哪些工作
    * 是否遇到困难
    * 今天计划做哪些工作
    * 这张卡还需要多久可以完成
  * 原则
    * token ： 站会时，大家围城一个圈，手持token，发言结束后顺时针传递token。没有token的人不能发言，需要发言申请token
    * 简短 ：尽量保持简短，涉及技术细节的可另约时间或者会议沟通
    * 参会人员 ： 全体

#### Retro
  * 频率 ： 每个迭代的最后一个工作日
  * 时长 ： 30 - 60分钟（根据团队大小进行调节）
  * 内容
    * 由retro主持预定会议室以及时间，发送邀请邮件给全体组员
    * 会议开始前主持在白板上列出"Well"、"Less Well"、"Suggestion"和"Action"
    * 每个人以便签的形式，写出自己的想法、贴到对应的主题下
    * 参会人员为白板上的内容投票，由时间决定按照票数前几项进行讨论
    * 讨论的结果形成Action，每个Action需要找到对应的owner
    * 下次Retro首先要列出上次Action，并且逐步检查完成
  * 原则
    * 会议主持人的选取采用轮换制
    * 一个便签上只表达一个想法
    * 每个Action都需要有人认领
    * 会议结束后，会议主持人负责拍照总结到wiki上，并发送邮件给全体组员
  * 参会人员 ： 全体
  * Demo
    ![](/images/agile-software-development/retro-demo.png)

#### ShowCase
  * 频率 ： 每个迭代的最后一天或者新迭代开始的第一天
  * 时长 ： 20 - 30分钟（根据团队大小进行调节）
  * 内容 ： 为产品经理或者客户演示本迭代完成的产品功能
  * 参会人员 ： 全体

#### Code Review
  * 频率 ： 每天、每个Story（按照团队的规则）
  * 时长 ： 30分钟
  * 内容
    * 快速代码走查
    * 上一次review的反馈跟踪
  * 参会人员 : DEV

#### IPM
  * 频率 ： 每个迭代开始的第一天
  * 时长 ： 20 - 40分钟（根据团队大小进行调节）
  * 内容
    * BA将下个迭代的所有story列出（描述和验证标准）
    * 全组成员需共同理解每个story所要完成的产品功能，并对story进行估点
    * BA及时更新story信息到物理卡墙和jira(BOSS)上，同事更新迭代计划和发布计划
  * 参会人员 ： 全体

## Story Wall

![](/images/agile-software-development/story-wall.png)

#### Story Analysis
  * BA 在分析过程中的Story
  * Story=>Done（Story的描述和AC与产品确认好，卡已经更新到wiki上）

#### Story Done
  * BA 已经分析好的Story
  * Story Done => To Do（迭代启动会之前，本迭代计划的卡放到To Do里面）

#### ToDo
  * 这个迭代中需要做的Story
  * 一般To Do里面的卡是按照优先级来排列的
  * To Do => Doing
    * 开发有权利去挪卡
    * 挪卡后在物理卡上更新做卡的人
    * jira备注上更新start时间以及做卡的人
    * kill off

#### Doing
  * 已经有人在做的卡
  * Doing => Integration Test
    * A3Report、Code Review
    * 需要找测试一起过mini showcase

#### Waiting For Testing
  * 等待联调阶段
  * 当Dev（前端或者后端）开发完成，将Story移动到Waiting For Testing，等待联调
  * Waiting For Testing => Integration Testing 需要前后端相互确认可以联调条件Ready，可以进入到联调阶段

#### Integration Testing
  * 改Story正处于联调阶段
  * Integration Testing => Testing (联调成功后，和测试、BA一起过mini showcase)

#### Testing
  * 联调成功，等待测试详细的测试
  * Testing => Acceptance (测试测完OK后放入Accptance)

#### Acceptance
  * 测试已通过，等待产品经理验收
  * Acceptance => Done (产品验收成功)

#### Done
  * 产品经理验收成功，Story结束

## 沟通
  ![](/images/agile-software-development/agile.jpg)

## 敏捷开发宣言

 > **个体和互动** 高于 流程和工具

 > **工作的软件** 高于 详尽的文档

 > **客户合作** 高于 合同谈判

 > **响应变化** 高于 遵循计划

* 也就是说，尽管右项有其价值，我们更重视左项的价值。   
