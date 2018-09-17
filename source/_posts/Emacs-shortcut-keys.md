---
title: Emacs shortcut keys
date: 2018-09-10 18:31:18
tags: 
  - emacs
  - shortcut
  - 快捷键
categories: Emacs
---
> 基于自己常用的的快捷键，整理了这份list，不定期更新～
### ** 基本命令
```
C-x C-f        打开/新建文件
C-x C-s        保存当前缓冲区
C-x C-w        当前缓冲区另存为
C-x i          光标处插入文本
C-x b          切换buffer
C-x C-b        显示buffer列表
C-x k          关闭当前buffer
C-x C-c        关闭emacs
```
### ** 光标移动命令
```
C-f            前进一个字符
C-b            后退一个字符
C-p            上一行
C-n            下一行
C-a            光标移动到行首
C-e            光标移动到行尾

M-f            前进一个单词
M-b            后退一个单词
M-a            光标移动到句首
M-e            光标移动到句尾
M-<            光标移动到全文开始
M->            光标移动到全文结尾
```
<!--more-->
### ** 编辑命令
```
C-d                   删除光标后一个字符

M-<DEL>               上次光标前一个单词
M-d                   删除光标后一个单词

C-k                   删除光标到行尾处
M-k                   删除光标到句尾处

C-/ | C-_ | C-x u     撤销修改
```

### ** 高效
C-u number command  重复number次命令，比如
```
C-u 8 * => ********
C-u 8 C-f => 向前移动8个字符
```

### ** 命令工具
```
C-g            停止当前命令
```