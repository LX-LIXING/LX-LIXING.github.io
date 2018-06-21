---
layout: post
title:  "position 定位属性"
date:   2018-06-21
categories: css
tags: css
excerpt: 学习css中position属性以及学习中涉及到的其他概念。
---

* 文档结构
{:toc}

学习css中position属性以及学习中涉及到的其他概念（块级元素、内联元素、文档流）

---

# position 定位属性

## 	1 相关概念

### 1.1 html的块级元素和内联元素

- 块级元素

	元素：

		div , dl , form , h1 , h2 , h3 , h4 , h5 , h6 , menu , ol , p , table , ul , li, address , center

	特点：

		一、总是在新行上开始，占据一整行；
		二、高度，行高，内外边都可控制；
		三、宽度始终是与浏览器宽度一样，与内容无关；
		四、它可以容纳内联元素和其他块元素。

- 内联元素

	元素：

		a , b , br , em , font , img , input , label , select , small , span , textarea 

	特点：

		一、和其他元素都在一行上；
		二、行内元素只能容纳文本或者其他行内元素。
		三、宽度只与内容有关；
		四、高，行高及外边距和内边距部分可改变；

		不可以设置宽高，其宽度随着内容增加，高度随字体大小而改变，内联元素可以设置内、外边界，但是内、外边界不对上下起作用，只能对左右起作用

- 可变元素：

	元素：button

	特点：根据上下文关系确定该元素是块元素还是内联元素

- css改变元素类型
	相关属性：

		display:block  -- 显示为块级元素
		display:inline  -- 显示为内联元素
		display:inline-block -- 显示为内联块元素，表现为同行显示并可修改宽高内外边距等属性

	其他属性：
		display:none  -- 隐藏元素，可以改变none为block显示出来元素

- 注意

		内联元素的margin-left / margin-right及padding-left / padding-rigtht是可以控制的，所以可以通过这4个属性来控制内联元素的宽度。

		内联元素的内部也可以放块级元素标签，而且内部的块级元素标签会撑大外部的内联标签，所以可以通过放块元素来控制内联元素的高度
		
### 1.2 HTML文档流

- 概念：将窗体自上而下分成一行一行,并在每行中按从左至右的挨次排放元素,即为普通流/文档流。

- 对文档流影响

		1.position的值为absolute、fixed的元素脱离文档流，static、relative没有脱离文档流
		2.float
		

##  2 介绍说明

### 2.1 说明
	position 属性规定元素的定位类型，相对定位元素会相对于它在正常流中的默认位置偏移

### 2.2 position属性值

- absolute：绝对定位，完全离开文档流。

		1. 使用left，right，top，bottom等属性相对于其最接近的一个具有定位设置（即position值不为默认值static）的父对象进行绝对定位。
		2. 如果不存在这样的父对象，则依据body对象。而其层叠通过z-index属性定义。
		3. 当对象定位在浏览器窗口以外，浏览器因此显示滚动条。
		4. 因为不在流中元素可以重叠在其他元素只上，只是相对于父对象做偏移。

- relative：相对定位，原本所占的空间仍保留。

		1. 对象不可层叠，但将依据left，right，top，bottom等属性在正常文档流中偏移位置。
		2. 当对象定位在浏览器窗口以外，浏览器因此显示滚动条。
		3. 因为在文档流中，位置会受到文档中元素的影响，不可与流中元素重合。

- fixed：固定定位，完全离开文档流。

		1. 对象定位遵从绝对(absolute)方式，相对于浏览器窗口进行定位。
		2. 当对象定位在浏览器窗口以外，浏览器不会因此显示滚动条，而当滚动条滚动时，对象始终固定在原来位置，即相关于视区进行偏移。
		3. 因为不在流中元素可以重叠在其他元素上。

- static：元素框正常生成。

		1. 块级元素生成一个矩形框，作为文档流的一部分。
		2. 行内元素则会创建一个或多个行框，置于其父元素中。

- inherit：继承值，对象将继承其父对象相应的值。

- sticky：在一个内容中，我们可以固定一个部分，然后到了另一个内容，又会固定另外一个部分

	1.代码

```

		<!DOCTYPE html>
		<html lang="en">
		<head>
		    <meta charset="UTF-8">
		    <title>alink</title>
		    <style>
		        body {
		            margin: 0;
		            padding: 0;
		        }
		        .wrap {
		            border: 20px solid blue;
		        }
		        .header {
		            position: sticky;
		            top: 20px;
		            border: 20px solid red;
		            margin-top: 20px;
		        }
		    </style>
		</head>
		<body>
		    <div class="wrap">
		        <div class="header">
		            这是头部
		        </div>
		        <div class="content">
		            第一部分<br>
		            第一部分<br>
		            第一部分<br>
		            第一部分<br>
		            第一部分<br>
		            第一部分<br>
					
					...多复制几行看效果
					
		        </div>
		    </div>
		    <div class="wrap">
		        <div class="header">
		            这是另一个头部
		        </div>
		        <div class="content">
		            第二部分<br>
		            第二部分<br>
		            第二部分<br>
		            第二部分<br>
		            第二部分<br>

					...多复制几行看效果
					
		        </div>
		    </div>
		</body>
		</html>

```

## 3 z-index 控制元素

- 声明：z-index只能在position属性值为relative或absolute或fixed的元素上有效。
- 原理：z-index的值可以控制定位元素在垂直于显示屏幕方向（z轴）上的堆叠顺序,值大的元素发生重叠时会在值小的元素上面。
- 实例

	1. 父元素不设置z-index时，子元素z-index值大于0时，在父元素上，子元素z-index值小于0时，在父元素下。
	2. 父元素设置z-index时，子元素在父元素上。

## 4 浮动

	浮动元素会脱离文档流并向左/向右浮动，直到碰到父元素或者另一个浮动元素

