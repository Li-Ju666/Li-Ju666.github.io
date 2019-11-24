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

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;border-color:#ccc;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#fff;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:#ccc;color:#333;background-color:#f0f0f0;}
.tg .tg-cly1{text-align:left;vertical-align:middle}
.tg .tg-nrix{text-align:center;vertical-align:middle}
</style>
<table class="tg">
  <tr>
    <th class="tg-nrix">00</th>
    <th class="tg-nrix">01</th>
    <th class="tg-nrix">10</th>
    <th class="tg-nrix">11</th>
    <th class="tg-cly1"></th>
  </tr>
  <tr>
    <td class="tg-nrix" colspan="2">14</td>
    <td class="tg-nrix">15</td>
    <td class="tg-nrix">15</td>
    <td class="tg-cly1"></td>
  </tr>
  <tr>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
  </tr>
  <tr>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
    <td class="tg-cly1"></td>
  </tr>
</table>
