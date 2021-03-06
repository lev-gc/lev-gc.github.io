---
title: Linux命令行常用快捷键
date: 2016-05-15 10:57:00
tags: [Linux]
---

## **光标位置移动**

| Shortcut                   | Remark                             |
| -------------------------- | ---------------------------------- |
| **Ctrl+A*                  | 移动到行首|
| **Ctrl+E*                  | 移动到行末|
| Ctrl+B                     | 向左边移动一个字符，同Left键|
| Ctrl+F                     | 向右边移动一个字符，同Right键|
| **Ctrl+Left & Ctrl+Right*  | 以单词为单位左右移动|
| Alt+B/Esc to B             | 向左边移动一个单词|
| Alt+F/Esc to F             | 向右边移动一个单词|

## **编辑命令**

| Shortcut                   | Remark                             |
| -------------------------- | ---------------------------------- |
| **Ctrl+L*                  | 清屏操作，作用同输入clear/reset|
| **Ctrl+U*                  | 删除光标前的输入(配合Ctrl+Y可粘贴删除的内容)|
| **Ctrl+K*                  | 删除光标后的输入(配合Ctrl+Y可粘贴删除的内容)|
| Ctrl+Y                     | 粘贴被Ctrl+U/Ctrl+K 删掉的内容|
| Ctrl+W                     | 往光标左边删除单词|
| Alt+D/Esc to D             | 往光标右边删除单词|
| Ctrl+T                     | 交换光标左边的两个字符的位置|

## **查找历史命令**

| Shortcut                   | Remark                             |
| -------------------------- | ---------------------------------- |
| Ctrl+P                     | 显示当前命令的上一条命令，同UP键|
| Ctrl+N                     | 显示当前命令的下一条命令，同DOWN键|
| **Ctrl+R*                  | 开启历史命令搜索，Enter执行搜索结果，Esc显示搜索结果但不执行，再次输入Ctrl+R可以往前回溯符合s搜索条件的命令|
| **Ctrl+G*                  | 退出历史命令搜索|

## **控制命令**

| Shortcut                   | Remark                             |
| -------------------------- | ---------------------------------- |
| **Ctrl+S*                  | 阻止屏幕输出|
| **Ctrl+Q*                  | 允许屏幕输出|
| **Ctrl+C*                  | 终止命令，也可用于忽略当前输入直接跳到下一行|
| **Ctrl+Z*                  | 暂停命令，可以通过jobs查看作业，使用fg/bg加作业ID可前台/后台继续运行进程|

## **Bang命令**

| Shortcut                   | Remark                             |
| -------------------------- | ---------------------------------- |
| **!!*                      | 执行上一条命令|
| **!$*                      | 添加上一条命令的最后一个参数|
| !-n                        | 执行前n条命令|
| ^xxx                       | 删除上一条命令的xxx字符并执行|
| ^xxx^yyy                   | 替换上一条命令的xxx字符为yyy|
