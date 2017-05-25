---
title: Git多账号配置
date: 2016-05-22 16:52:00
tags: [Git]
---

在项目Repo里执行命令，会修改.git文件夹的config文件：
```
#git config user.name "newname"
#git config user.email "newemail"
```

取消全局设置：
```
#git config --global --unset user.name
#git config --global --unset user.email
```

如果需要使用多个Git账号，先取消Git的全局账号，然后在每个项目单独配置Name和Email。