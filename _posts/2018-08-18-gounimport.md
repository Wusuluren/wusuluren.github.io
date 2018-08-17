---
layout: default-blog-md
title: 自动移除go代码里未使用的package
category: markdown
---

使用Goland编写go代码时，IDE具有自动导入package的功能，写代码时很流畅，不用频繁跳转到文件头去插入导入package的语句，思路不会被打断。但是IDE没有自动删除未使用的package的功能，这样编译时会报错，需要手动去删除，这个体验就很糟糕了。为了能让写完代码的好心情不被这点小事弄烦，写了一个小程序来处理这件事，配合File Watcher插件，可以在保存文件后自动执行。效果如下图所示：  
<img src="{{ site.baseurl }}/static/image/2018-08-18/gounimport.gif" width="60%" height="auto">    
这样，写完代码后就行可以优雅地去喝（zhuang）茶（bi）了，代码在[这里](https://github.com/Wusuluren/some_code/blob/master/go/gounimport.go)。