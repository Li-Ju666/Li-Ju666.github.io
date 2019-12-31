---
layout:     post   				    # 使用的布局（不需要改）
title:      Optimization of Database				# 标题 
subtitle:    how database works
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

### Comparison between memory and disk
Memory is rapid for access, but it is expensive and volatile, which means the data will get lost when power is off or system crash occured. So memory is only used to operate data but usually not for data storage. 

Compared with memory, magnetic disk is slow but stable. So magnetic disk is mainly used to store the database. Whenever data need to be operated of access, magnetic disk passes the data to the memory. 

### Storage details about database in disk
In disk, the structure lies as this: bit -> byte -> block -> sector (physical unit) -> block (logical unit) -> disk (with a disk head for controlling). Database is actually a bunch of relations and generally each relation is stored as a seperate file, which occupies a series of blocks. In each file, a sequence of fixed-sized records/tuples are stored. 

In most cases, several records are stored in one block. As we stated before, a block is the logical unit of disk access, and access disk is far slow than memory operation, therefore we can conclude that the bottleneck of database performance is **the number of number of block transfer from disk to memory**. To optimize the performance of database, the key target is to reduce the operation on block transfer. **block factor**: *average* number of records in a single block (because the size of each tuple is not guaranteed to be fixed). 

### Record organization in file
In each file, records are stored in specific organization, which means where to store each record matters. Generally there are three ways to arrange records, including heap, sequential and hash. 

Heap: heap basicly means arrange them in no order: add new record whereever is free. This organization is efficient in data insertion but quite slow for data deletion and data searching. If a specific record requires to be delete, a linear search is mandatory. 

Sequential: sequential organization place records in the order of a certian attribute, implemented by a pointer chain. For search operation, binary search can be applied. For insertion, if there is free space in that block for the record, it can be saved there, otherwise an extra block named overflow block is required. The overloaded records are stored there and a pointer will point there. When deletion operation is required, besides deleting the record, the pointer chain also needs update. 

Hash: Generally the key field of the relation is used to hash the record. Each record will be stored in the memory at the address of hash function calculated. When two records are assigned to a same address, collision arises (which is rare in real life). For this situation, we have three ways: 1. using open address (if occupied, look for another address); 2. multiple hashing (a paricular method of open address); 3. chaining: a chaining list is used to store multiple values. 

However, to avoid collision, numerous buckets are needed. When they are empty they cannot be used by other application. To dynamically extend the hash table, we can use extendible hashing. Details about extendible hashing has been introduced in this article[https://li-ju666.github.io/2019/11/24/Extendible-Hashing-2019/]. 
