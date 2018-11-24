---
layout: default-blog-md
title: declare slice with three dots
category: markdown
---

在阅读一些go代码时，发现定义slice时，有时会使用3个点。比如下面这段代码（超级冲击波！）：
```go
func init() {
	// Generate layouts based on RFC 5322, section 3.3.

	dows := [...]string{"", "Mon, "}   // day-of-week
	days := [...]string{"2", "02"}     // day = 1*2DIGIT
	years := [...]string{"2006", "06"} // year = 4*DIGIT / 2*DIGIT
	seconds := [...]string{":05", ""}  // second
	// "-0700 (MST)" is not in RFC 5322, but is common.
	zones := [...]string{"-0700", "MST", "-0700 (MST)"} // zone = (("+" / "-") 4DIGIT) / "GMT" / ...

	for _, dow := range dows {
		for _, day := range days {
			for _, year := range years {
				for _, second := range seconds {
					for _, zone := range zones {
						s := dow + day + " Jan " + year + " 15:04" + second + " " + zone
						dateLayouts = append(dateLayouts, s)
					}
				}
			}
		}
	}
}
```
这种写法和常见的写法有什么不同呢？网上搜索下也没找到答案，决定写段代码测试一下，没想到Goland优秀的错误提示告诉了我，答案就是：  
<img src="{{ site.baseurl }}/static/image/2018-11-24/slice-without-dots.png" width="60%" height="auto">  
    
<img src="{{ site.baseurl }}/static/image/2018-11-24/slice-with-dots.png" width="60%" height="auto">    
哈哈，类型不一样。