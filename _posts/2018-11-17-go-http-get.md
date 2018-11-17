---
layout: default-blog-md
title: 探究go源码之http Get请求
category: markdown
---

golang标准库提供了很好用的http库，比如想要下载百度首页，只需要一行代码：    
```go
http.Get("http://www.baidu.com")
```
下面这张图片展示了内部的执行流程：  
<img src="{{ site.baseurl }}/static/image/2018-11-17/go-http-get.png" width="60%" height="auto">    
