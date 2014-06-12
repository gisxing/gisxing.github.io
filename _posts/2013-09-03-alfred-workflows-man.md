---
layout: post
title: "alfred workflows 编写笔记"
date: 2013-09-03 23:18
comments: true
categories: [alfred, workflows]
---

Alfred v2 添加了workflows的功能，花27镑买了一个family license , 成功推销掉所有license , 自己也花了点时间来研究一下这个workflows。

有点类似系统的automator功能，不过当然更强大，并且，可以自己写脚本来custom一翻，网上很多教程都只是略略带过就算，都说找几个workflows参详一下就明白，这个的确是，不过就当是学习笔记，还是把其中一些要点罗列一下。 

Alfred的workflows支持很多脚本语言，也可以直接跑sh script，我用的是python，可以直接在script那个输入框里面直接编辑python，也可以language选择 /bin/bash，
然后script中输入诸如 ： 

	python python-test.py -q {query}

注意Alfred中的脚本，或者action , trigger之类，都可以直接用{query}获取上一步骤的输出。 

*特别地，Script Filter中产生的用于回显在alfred中的xml格式，如果想用 open Url这个小控件直接选择时候打开，那么xml需要按照如下格式编写：

	<?xml version="1.0"?>
	<items>
	    <item uid="weekly" arg="http://css-weekly.com/issue-74/">
		<title>css周刊第74期</title>
		<subtitle>发布日期为:2013年08月29日 21:52:35</subtitle>
		<icon>2D5B3243-1F61-4C63-A2AB-320C51DC6FB2.png</icon>
	    </item>
	</items>

github上有很多alfred的module , 针对输出如上的xml有专门的包装，最常见的可能是feedback.py。
注意如果要使用'选中就打开url'这个控件的话，需要arg这个参数如上那样书写。 

feedback.py

	#author: Peter Okma
	import xml.etree.ElementTree as et


	class Feedback():
	    """Feeback used by Alfred Script Filter

	    Usage:
		fb = Feedback()
		fb.add_item('Hello', 'World')
		fb.add_item('Foo', 'Bar')
		print fb

	    """

	    def __init__(self):
		self.feedback = et.Element('items')

	    def __repr__(self):
		"""XML representation used by Alfred

		Returns:
		    XML string
		"""
		return et.tostring(self.feedback)

	    def add_item(self, title, subtitle="", arg="", valid="yes", autocomplete="", icon="icon.png"):
		"""
		Add item to alfred Feedback

		Args:
		    title(str): the title displayed by Alfred
		Keyword Args:
		    subtitle(str):    the subtitle displayed by Alfred
		    arg(str):         the value returned by alfred when item is selected
		    valid(str):       whether or not the entry can be selected in Alfred to trigger an action
		    autcomplete(str): the text to be inserted if an invalid item is selected. This is only used if 'valid' is 'no'
		    icon(str):        filename of icon that Alfred will display
		"""
		item = et.SubElement(self.feedback, 'item', uid=str(len(self.feedback)),
		    arg=arg, valid=valid, autocomplete=autocomplete)
		_title = et.SubElement(item, 'title')
		_title.text = title
		_sub = et.SubElement(item, 'subtitle')
		_sub.text = subtitle
		_icon = et.SubElement(item, 'icon')
		_icon.text = icon


可以看到add_item的用法，例如如下的XML输出:
	<item arg="{&quot;url&quot;:&quot;http://www.whoscored.com/Teams/26&quot;}" autocomplete="" uid="0" valid="yes">
		<title>Liverpool</title>
		<subtitle>England</subtitle>
		<icon>icon.png</icon>
	</item>

我们可以在后面接驳run script控件,继续使用一个python脚本

	from __future__ import print_function
	import json
	    
	temp = '{query}'

	response = json.loads(temp)
	url = response['url']

	print(url, end='')

这样就可以获取xml中arg中的url数据，并进行输出，紧接着可以使用copy to clipboard或者post notification等控件来进行后续操作，同样是使用{query} 来获取上一步的输出。


