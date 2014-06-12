---
layout: post
title: "正则表达式猫纸"
date: 2013-09-03 12:20
comments: true
categories: [re, 正则, 表达式]
---

(?!...) 
Matches if ... doesn’t match next. This is a negative lookahead assertion. For example, Isaac (?!Asimov) will match 'Isaac ' only if it’s not followed by 'Asimov'. 

例子
    re.match(r'.*0(?!-)','10--0-')
字符串中有 0 且后面没有跟带 - 的时候匹配。
