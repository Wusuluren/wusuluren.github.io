---
layout: default-blog-md
title: rust与数组越界
category: markdown
---

rust一向号称安全，编译器强大到能检测出内存问题。今天用最常见的数组越界问题来做了下实验，发现编译通过，运行时panic，如下图所示：  
<img src="{{ site.baseurl }}/static/image/2019-07-28/array_bounds.png" width="60%" height="auto">    
看来rust也不是解决bug的万能药啊。有些问题，也许时间是最好的解药。  
  
更新：
好吧，rust里可以通过get方法来获取一个Option结构，但这样写又很繁琐啊。还有，个人感觉rust写出来的代码可读性真的很差，一点也不“literature programming”。