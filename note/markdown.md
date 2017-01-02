大标题
======
小标题
------

# 标题1
## 标题2
### 标题3
#### 标题4
##### 标题5
###### 标题6

# 无序列表
* 一级
	* 二级
- 无序列表
	- 二级

# 有序列表
1. 一级1
	1. 二级1
2. 一级2
	2. 二级2
1. 一级3
	3. 二级2

文本
----
普通文本。
普通文本用`<br>`换行。<br>
这是第二行。

	普通文本，在行首用Tab或4个空格缩进。
	这是第二行。
	这是第三行

# 链接
[百度](http://www.baidu.com "百度主页")
[我的主页](http://wusuluren.github.io "我的主页")

# 字体格式
**粗体1**，__粗体2__<br>
*斜体1*，_斜体2_<br>
~~删除线1~~，**~~删除线1~~**，*~~删除线1~~*，***~~删除线1~~***<br>
分割线
***

# 引用
引用文本
>Hello Github
--wusuluren

多级引用
>第一级
>>第二级
>>>第三级

# 表格
|标题1|标题2|标题3|
| ----- |:------:| ------|
|内容1|内容2|内容3|

# 表情
emoji表情:blush:

# 代码
```cpp
printf("hello cpp");
```
```python
print("hello python")
```
```go
fmt.Println("hello go")
```
```shell
echo "hello shell"
```

# 流程图
```flow
st=>start: 开始
e=>end: 结束
op1=>operation: 选择
sub1=>subroutine: 分支
cond1=>condition: 是，否？
io=>inputoutput: 输入
st->op1->cond1
cond1(yes)->io->e
cond1(no)->sub1(right)->op1
```
