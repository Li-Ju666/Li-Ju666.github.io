---
layout:     post   				    # 使用的布局（不需要改）
title:      Extendable Hashing				# 标题 
subtitle:    #副标题
date:       2019-11-24 				# 时间
author:     Li Ju 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
mathjax: true
tags:								#标签
    database  
    data_structure
---


## Extendable Hashing
This is the problem in the assignment 1 of Database Design 2. Here are my solutions. 
### Problem Description
> Load the records: 2369, 3760, 4692, 4871, 5659, 1821, 1074, 7115, 1620, 2428, 3943, 4750, 6975, 4981, 9208 (in the given order) into an
expandable hash file based on extendible hashing. Show the structure of the directory at each step. Show the directory at each step, and the global and local depths. 
Use the hash function h(k) = K mod 32. Assume that each bucket is one disk block and holds two records. 

### Problem Solution
The key feature of extendible hashing is: extendible as the number of data to be stored increases. 

<escape>
<table>
  <tr>
    <th>00</th>
    <th>01</th>
    <th>10</th>
    <th>11</th>
  </tr>
  <tr>
    <td colspan="2">14</td>
    <td>15</td>
    <td>15</td>
  </tr>
</table>
</escape>
