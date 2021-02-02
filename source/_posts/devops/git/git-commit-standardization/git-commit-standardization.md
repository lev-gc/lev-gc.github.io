---
title: Git Commit规范化
date: 2019-02-06 12:20:00
tags: [Git,Lint]
---

> 为了方便回溯历史、进行Merge和cherry-pick等，我们需要Git仓库的历史树具有较强的可读性，因此需要规范化**commit message** 和 **commit方式**。

## Commit message

commit message是用于正确地简述该提交所涉及的内容，按照一定的规范撰写能有效地提高团队的工作效率。撰写的最基本格式可以按照`<type>: <subject>`这种模式，其中`type`是提交内容的类型，而`subject`则是提交的概要描述。如果概要描述`subject`无法清楚描述本次提交的内容，则可以在换行后的`<body>`部分撰写详细的Description。

更多详细的扩展模式可以参考[commitizen](https://github.com/commitizen/cz-cli)提供的选项，这种更精细化的标准提供了更多的内容分类，包括提交代码的影响范围、关闭issue等。

常用的`type`有以下这些：

- feat: 增加新功能
- fix: bug的修复
- docs: 文档的添加或修改
- style: 不影响代码功能的代码格式变化
- refactor: 代码的重构
- perf: 提升性能的代码
- test: 增加测试用例
- build: 项目构建相关的代码
- ci: 持续集成相关的代码
- chore: 其他一些不影响源码的提交
- revert: 提交的回滚

> 参考：[Git: 教你如何在Commit时有话可说](https://mp.weixin.qq.com/s?__biz=MzAwNDYwNzU2MQ==&mid=401622986&idx=1&sn=470717939914b956ac372667ed23863c&scene=1&srcid=0114ZcTNyAMH8CLwTKlj6CTN#rd)

### 相关规范工具

- 命令行工具[commitizen](https://github.com/commitizen/cz-cli)；
- Intellij IDEA插件：**Git Commit Template**，JetBrains全家桶通用；

## Commit方式

关于Commit方式同样需要规范化：

1. 清晰描述commit的内容，不同类型或不同功能的内容 __分开多次提交__ （IDEA或者VSCode都支持单个文件选择部分内容进行stage和commit）；
2. 根据commit message提交文件，不掺杂commit message描述以外的内容；
3. 同一个文件的移动文件夹操作与重命名或者文件内容大量修改操作需要尽量分开进行commit，为了能识别文件变化操作是移动而不是删除文件并新增新的文件，这样能尽量保存文件的修改历史；
