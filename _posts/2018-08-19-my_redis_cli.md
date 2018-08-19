---
layout: default-blog-md
title: 让redis-cli的输出更易读
category: markdown
---

redis-cli的输出结果里不会区分数据类型，这样看着有点费劲。所以萌生出一个想法，对于hash类型的数据采用更易读的方式来显示，并且对于list、hash、set、zset这几种类型数据的数据，可以用不同的颜色来区分。  
想要实现上面说的功能，需要修改的是redis-cli.c的代码。redis-cli支持三种输出方式：raw、tty和csv，一般我们使用的都是tty输出。实现tty输出的函数是cliFormatReplyTTY，在这个函数里，会判断redisReply的type，对于复杂的数据类型，type是REDIS_REPLY_ARRAY，可以看出来，redis-cli并没有刻意分区，复杂数据类型都是按照数组来处理的。那么我要修改的主要是这里的判断，改成如下：  
```c
    case REDIS_REPLY_ARRAY:
		if (config.last_cmd[0] == 'h' && strcmp(config.last_cmd, "help") && config.pretty) {
			out = cliPrettyFormatReplyArrayTTY(r, out, prefix);
		} else {
			out = cliFormatReplyArrayTTY(r, out, prefix);
		}
    break;
```
为了能记录当前的命令，我给config这个全局变量添加了一个字段last_cmd，如果last_cmd是以‘h’字母开头的，就当成是hash类型的命令，这个判断有点太粗糙了，不过一般够用。对于hash类型的命令，我用自己的实现cliPrettyFormatReplyArrayTTY来处理，其它的用默认的方法去处理，只不过是提取成了cliFormatReplyArrayTTY函数。  
为了用颜色来区分输出结果，添加了两个函数cliColorful和cliAutoColorful，在cliPrettyFormatReplyArrayTTY和cliFormatReplyArrayTTY函数里进行调用。在终端输出带颜色的字符串是shell本来就支持的，而且redis-cli里对lua脚本支持颜色，所以实现就很方便了，cliColorful函数如下所示:  
```c
static char *cliColorful(int color, int bold, char *s) {
	char *o = sdsnew("");
	o = sdscatfmt(o,"\033[%i;%i;49m",bold,color);
    o = sdscat(o,s);
    o = sdscat(o,"\033[0m");
	return o;
}
```  
cliAutoColorful函数类型，不过会对last_cmd的首字母进行判断，来区分数据类型。  
另外为了使用方便，为config变量添加了两个字段：pretty和color。用来做命令行选项，前者可以把hash类型的输出美化，后者可以使用不同的颜色区分输出。在命令行查看帮助时会多出如下两行：  
```
  --pretty           Use pretty formatting for replies print.
  --color  		  Use colorful formatting for replies print.
```
在命令行输入`./redis-cli --color --pretty`，就可以使用新的redis-cli程序，效果如下图所示：  
<img src="{{ site.baseurl }}/static/image/2018-08-19/myredis-cli.PNG">    
