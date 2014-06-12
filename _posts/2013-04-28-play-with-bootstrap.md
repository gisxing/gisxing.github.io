---
layout: post
title: "Bootstrap 初体验"
date: 2013-04-28 15:05
comments: true
categories: 
---

在官方网站上下载最新的源代码 2.3.1 版本
http://twitter.github.com/bootstrap/

简单建立一个页面测试一下效果

	<!DOCTYPE html>
	<html lang="en">
	<head>
	     <script src="jQuery.js"></script>
	     <script src="bootstrap.js"></script>
	     <link href="bootstrap.css" rel="stylesheet">
	</head>

可以看到是基于JQuey的库， 框架文件还包括两个文件： bootstrap.js bootstrap.css
仅此而已！

<body>

	<body>
	     <div class="container-fluid">

		 <div class="row-fluid">

		      <div class="span3">
			   <div class="well sidebar-nav">
			       <ul>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
				    <li><a href="#">link</a></li>
			       </ul>
			   </div>
		      </div>

		      <div class="span9">
			   <div class="hero-unit">
			       <h1>Welcome to HTML5!</h1>
			   </div>

			   <div class="row-fluid">
				    <div class="span4">
				      <h2> 1</h2>
				      <p>HTML5 Introduction... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>

				    <div class="span4">
				      <h2> 2</h2>
				      <p>HTML5 Course... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>

				    <div class="span4">
				      <h2> 3</h2>
				      <p>HTML5 Drag... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>
			   </div>

			   <div class="row-fluid">
				    <div class="span4">
				      <h2> 4</h2>
				      <p>HTML5 Introduction... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>

				    <div class="span4">
				      <h2> 5</h2>
				      <p>HTML5 Course... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>

				    <div class="span4">
				      <h2> 6</h2>
				      <p>HTML5 Drag... </p>
				      <p><a class="btn" href="#">View details &raquo;</a></p>
				    </div>
			   </div>

		      </div>

		 </div>

	     </div>

	</body>

看看实际效果: <http://bilibala.net/bootstrap_test.html>


