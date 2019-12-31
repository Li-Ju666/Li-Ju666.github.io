---
layout:     post   				    # 使用的布局（不需要改）
title:      Optimization of Database				# 标题 
subtitle:    #副标题
date:       2020-01-01 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database
---

# Optimization of Database
In this article, the mechanism behind the database together with its optimization will be introduced. 
## Basic Knowledge about Storage
Basically, there are two kinds of storage in computer, memory and magnetic disk. 

Memory is rapid for access, but it is expensive and volatile, which means the data will get lost when power is off or system crash occured. So memory is only used to operate data but usually not for data storage. 

Compared with memory, magnetic disk is slow but stable. So magnetic disk is mainly used to store the database. 

In disk, the structure lies as this: bit -> byte -> block -> sector (physical unit) -> block (logical unit) -> disk. 

Whenever data need to be operated of access, magnetic disk passes the data to the memory. 
