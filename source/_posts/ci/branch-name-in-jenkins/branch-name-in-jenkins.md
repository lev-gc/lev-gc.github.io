---
title: Jenkins项目中配置Git分支
date: 2018-11-26 20:04:00
tags: [CI,Jenkins]
---

> 最近发现很多人在Jenkins上的项目构建配置中都是用`*/xxx`这种方式来指定branch的，分享一点相关的经验。

#### 问题
当我们在Jenkins上新添加一个构建项目时，项目的默认配置中的`Branches to build`（也就是需要构建的目标分支）就是写着`*/master`，所以大部分不熟悉的使用者会选择像下图里一样直接把后面的`master`改成实际需要构建的branch name，而无视掉前面的`*/`。虽然在绝大多数情况下这样的配置是会正常工作不出问题，但是很明显配置中的`*`是起通配符作用的，这样干明显就是在给自己藏雷。  

![Jenkins branch](https://raw.githubusercontent.com/lev-gc/lev-gc.github.io/source/source/_posts/ci/branch-name-in-jenkins/jenkins_branch.png)

#### 案例
曾经遇到过的一个真实情况，GitLab上同时存在这样的两个branch：常驻branch `develop`和临时branch `xxx/develop`，Jenkins设置了`Branches to build`为`*/develop`，通过pollSCM的方式两分钟扫描一次，发现分支有变化就执行构建，因为每次都扫描到了这两个branch，所以每次都会执行构建，最后一个晚上下来构建达到了几百次。当然，即使不构建几百次，在一些情况下单纯是构建发布了非期望的branch这个事情其实就够糟糕的了。

####  解决方案
- 第一种方案，建议直接写branch name，不要在前面加`*/`。这个问题坑过很多人，Jenkins的官方JIRA上能找到一堆跟这个问题相关的Issue。
- 第二种方案，不使用pollSCM的触发方式。如今Jenkins拥有众多的插件，如果需要即时构建最新代码，除了pollSCM的触发方式外，其实更推荐使用对应Git托管服务（GitHub、GitLab等）的插件，通过`WebHook`的方式，由代码托管服务器来通知Jenkins进行构建，这样便能完全避开因为Jenkins扫描代码分支导致的问题。