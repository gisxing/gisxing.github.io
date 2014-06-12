---
layout: post
title: "vi 猫纸"
date: 2013-07-21 01:02
comments: true
categories: 
---

find 的时候不分大小写:
/findstr\c

检视文件编码:
:set fileencoding

以下命令可以对标点内的内容进行操作。

ci'、ci"、ci(、ci[、ci{、ci< - 分别更改这些配对标点符号中的文本内容

di'、di"、di(或dib、di[、di{或diB、di< - 分别删除这些配对标点符号中的文本内容

yi'、yi"、yi(、yi[、yi{、yi< - 分别复制这些配对标点符号中的文本内容

vi'、vi"、vi(、vi[、vi{、vi< - 分别选中这些配对标点符号中的文本内容
