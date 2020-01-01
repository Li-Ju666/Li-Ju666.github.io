---
layout:     post   				    # 使用的布局（不需要改）
title:      Optimization of Database II				# 标题 
subtitle:   Index structure of database
date:       2020-01-01 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database
---
In this series of articles, the mechanism behind the database together with its optimization will be introduced. 

# Index Structure
This article will introduce how various kinds of indexes are arranged and how they connect with physical records. 

From the [1st article](https://li-ju666.github.io/2020/01/01/Optimization-of-database/) of this series, we know that
in most cases, records in a file/relation are ordered/hashed by one or several specific attributes so that we can access them
in a rapid way. How can these ordered attribute achieve a fast access and if we want to access records via other attributes, 
how can we make it faster? Actually it is index that achieve this, and we can create more than one index to offer several
paths to access our database. 

**Index:** a file storing a/several attribute(s)' values and memory address(es) of their corresponding record(s) . 

## Different Types of Indexes: 
1. Primary Index: Primary index is a file storing 1: each record's key value(which is unique for each record) K(i) and 2: the pointer to the record's disk 
address P(i). For primary index, the physical order of records is exactly the same as the order of records in primary
index. 

2. Clustering Index: Clustering index is a file storing 1: each non-unique attribute's distinct values A(i) and 2: the pointer 
to the value's corresponding disk address P(i) (Only one address though there may be several records). For clustering
index, the physical order of records is also exactly the same as the order of records in clustering index. 

3. 

For primary index and clustering index, we can generally call them first index, because the physical order of records
are the same as their order in the index. The file is only possible to follow one order, therefore, there is at most 
*ONE* first index. **For first index**, we could store them sparsely or densely, which it called sparse index or 
dense index. The word sparse or dense means: if we store every key-address (value-address) pair in the index. 
As the file is ordered by the key/value, with part of key-address pairs, we still access them with slightly more cost. 

