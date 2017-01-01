## 数据类型
- 整数
    - 没有大小限制
- 浮点数
    - 没有大小限制
- 字符串
    - 转义字符
    - `r''`表示内部的字符串默认不转义
    - `'''A...B'''`表示多行内容，A后面从新行起
- 布尔值
    - True和False
    - 可以用and、or、not运算
- 空值
    - None，不等同于0
- 变量
    - 变量名命名规则同C语言
    - 变量本身类型不固定
- 常量
    - 表面意义，实际等同变量

## 字符串和编码
- 字符编码
    - Unicode是定长编码，每个字符占2字节
    - UTF-8是变长编码，兼容ASCII编码
    - 在计算机内存中使用Unicode编码，在网络和硬盘上使用UTF-8编码
    - 对于单字符，可以使用ord()、chr()
    - python的字符串类型是str，在内存中以Unicode表示，如果要在网络和硬盘上使用，就要变成以字节为单位的bytes
    - 对bytes类型的数据，带b前缀
    - 计算机内存中以Unicode表示的str通过encode()编码为bytes，encode可以指定编码格式为ascii或utf-8
    - 网络和硬盘上的字节流为bytes，可以通过decode()编码为str
    - 计算str包含多少字符，使用len()函数
    - 为避免乱码，应坚持使用UTF-8编码对str和bytes进行转换
    - python源码开头格式如下
```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
```
    - 文本编辑器应使用`UTF-8 without BOM`编码
- 格式化
    - 接近C语言，举例
`'%%Hi, my name is %s. I am %d years old' % (houwt, 18)`

## 使用list和tuple
- list
    - 列表list是一种有序的集合，可以随时添加和删除其中的元素。举例
`classmates = ['a', b'', 'c']`
    - 长度用len()获得
    - 按数组方式访问，索引从0开始，-1表示最后一个元素
    - append追加元素到末尾
    - insert插入元素到指定位置
    - pop删除末尾或指定位置的元素
    - list里面元素的数据类型可以不同，也可以嵌套
- tuple
    - 有序列表，一旦初始化就不能修改
    - 意义：tuple不可变，代码更安全
    - 定义只有一个元素的tuple的方法：`t = (1,)`
    - tuple内可以包含可变的list

## 条件判断
- if语句
    - 范例
```python
if <条件判断1>:
    <执行1>
elif <条件判断2>:
    <执行2>
else:
    <执行3>
```

## 循环
- for...in循环
    - 范例
```python
for x in list(rang(5)):
    print x
```
- while循环
    - 范例
```
while True:
    print 'hello'
```

## 使用dict和set
- dict
    - 也称map，使用键-值（key-value）存储，具有极快的查找速度。
    - 举例
```python
d = ['Michael':95, 'Bob':75, 'Tracy':85]
d['Michael']
95
d['Bob'] = 67
#判断key是否存在
'Thomas' in d
d.get('Thomas')
d.get('Thomas', -1)
#删除一个key
d.pop('Bob')
```
    - dict的特点
        - 查找和插入速度快
        - 占用内存多
    - dict的key必须是不可变对象，包括字符串、整数等，而list是可变的。
- set
    - 和dict类似，也是一组key的集合，但不存储value。
    - 在set中，没有重复的key，重复元素自动被过滤。
    - 创建set，需要提供一个list作为输入集合。
    - 举例
```python
s = set([1, 2, 3])
#s = {1, 2, 3}
s.add(4)
s.remove(4)
```
    - set可以做数学意义上的交集（&）、并集（|）。
- 不可变对象

## 函数
- 调用函数
    - 查看帮助：help(abs)
    - 数据类型转换：int、float、str、bool
- 定义函数
    - 举例
```python
# abstest.py
# 导入函数form abstest import my_abs
def my_abs(x):
    if x >= 0:
        return x
    else:
        return -x
    return None
``` 
```python
# 空函数
def nop():
    pass
```
    - 参数检查：参数个数不对，python解释器可以检查出来；参数类型不对，python解释器检查不出来。
    - 返回多个值，实际返回的是一个tuple
- 函数参数
    - 位置参数（必选参数）
    - 默认参数，放在位置参数后面
    - 可变参数：list、tuple、*
    - 关键字参数：dict、**
    - 命名关键字参数：特殊分隔符*，类似位置参数
    - 参数组合：参数定义顺序同上，但关键字参数在最后
- 递归函数
    - python解释器没有对尾递归做优化，存在栈溢出问题。

## 高级特性 
- 切片
    - 应用于list和tuple，截取某个范围的片段。
- 迭代
    - 通过`for ... in`来迭代，应用于任何可迭代的对象，包括list、tuple、dict、str等。
    - enumerate函数
- 列表生成式
    - 符号[]，举例
```python
list(range(0, 10))
[ x*x for x in range(1, 10)]
```
- 生成器
    - 一边循环一边计算，符号()
    - 通过next函数获得下一个返回值
    - 函数定义中包含yield关键字，是生成器函数。每次函数执行时，遇到yield返回，下次接着执行。
- 迭代器
    - 可迭代对象：Iterable
    - 使用isinstance函数判断一个对象是否是可迭代对象
    - 迭代器：Iterator
    - 生成器都是迭代器对象
     - iter函数

## 函数式编程
- 特点
    - 抽象程度高，不使用变量；没有副作用，输入确定，输出确定；函数可以作为参数和返回值
    - python允许使用变量，不是纯函数式编程

### 高阶函数
- 特点
    - 变量可以指向函数
    - 函数名也是变量，可以被修改
    - 传入函数：函数当参数
- map/reduce
    - map函数：接收两个参数，一个是函数，一个是Iterable，map将传入的函数依次作用到序列的每个元素，并把结果作为新的Iterator返回
    - reduce函数：接收两个参数，一个是函数（必须有两个以上参数），一个是Iterable，把函数作用在序列上，reduce把结果继续和序列的下一个元素做累积计算
- filter
    - filter函数：接收两个参数，一个是函数，一个是Iterable，filter将传入的函数依次作用到序列的每个元素，然后根据返回值是True还是False决定保留还是丢弃该元素
- sorted
    - 排序，接收1-3个参数，分别是序列、key、reverse

### 返回函数
- 函数作为返回值。内部函数可以引用外部函数的参数和局部变量，相关参数和变量都保存在返回的内部函数，这种方式叫“闭包”
- 闭包，返回函数不要引用局部变量

### 匿名函数
- 关键字lambda表示匿名函数，举例`lambda x : x * x`
- 只能有一个表达式，不用写return
- 匿名函数可以当参数和返回值

### 装饰器
### 偏函数
- 在functools模块内，functools.partial
- 作用：把一个函数的某些参数固定（设置默认值），返回新函数

## 模块
- 一个py文件就是一个模块，一个含有__init__.py的目录是一个包

### 使用模块
- 作用域：`xxx`和`__xxx__`是公开的，`_xxx`和`__xxx`是非公开的

### 安装第三方模块
- pip

## 面向对象编程
数据封装、继承和多态

### 类和实例
- object：基类
- self：this指针，类内部使用属性也要用self修饰
- __init__：初始化函数

### 访问限制
- 类的属性名称前加双下划线__，就变成了私有变量，外部不能访问。但可以使用ClassInst._ClassName__AttrName来访问（可能不兼容）
- 特殊变量：形式为__xxx__，保留名称，公开的

### 继承和多态
- 子类获得父类的全部属性，为继承
- 子类可以覆盖父类的属性，为多态
- 开闭原则
	- 对扩展开放：允许新增子类
	- 对修改封闭：不需要修改依赖父类类型的函数
- 动态语言的鸭子类型：不要求严格的继承体系

### 获取对象信息
- type、isinstance、dir、getattr、setattr、hasattr
- len等函数实际调用的是类自己的__len__函数

### 实例属性和类属性
- 实例属性会覆盖类属性

## 面向对象高级编程
- 多重继承、定制类、元类等概念

### 使用__slots__
- 可以给类和实例动态绑定属性和方法
- __slots__可以限制实例的属性

### 使用@property

### 多重继承
- 多重继承可以避免复杂的多层次的单继承链

### 定制类
- python的class允许定义很多定制方法，以生成特定的类

## 枚举类
- 使用`from enum import Enum`导入模块，枚举类成员具有value和name属性

### 元类

## 错误、调试和测试

### 错误处理
- try...except...finally...机制
- 错误也是类
- 记录错误：logging模块
- raise抛出错误

### 调试
- print
- assert：可以控制关闭
- logging：允许指定记录信息的级别
- pdb：调试器

### 单元测试
- unittest模块
- setUp和tearDown

### 文档测试
- doctest模块

## IO编程
- 同步IO和异步IO。异步IO涉及到回调函数、轮询等概念

### 文件读写
- IOError
- with语句open文件自动调用close函数

### StringIO和BytesIO
- file-like Object

### 操作文件和目录
- os模块，os.path和shutil模块

### 序列化
- pickle模块，把内存对象传输到硬盘或网络
- json模块，JSON编码是UTF-8

## 进程和线程

### 进程
- os.fork只能用在unix系的系统
- multiprocessing模块是跨平台的进程。Process、Pool
- 子进程：subprocess模块
- 进程间通信

### 线程
- _thread模块和threading模块。前者低级，后者高级
- 多线程需要上锁
- 由于python解释器的GIL全局锁，导致多线程无法利用多核，但多进程不受影响

### ThreadLocal

### 进程 vs线程
- 多进程不会互相影响，一个挂掉，别人没事
- 多线程互相影响，一个挂掉，全部都挂
- 计算密集型适合c等语言；IO密集型适合python等语言，比如web应用
- 异步IO：事件驱动模型，协程

### 分布式进程
- managers模块

## 正则表达式
- 基本匹配模式
- re模块：match、split、group、compile

## 常用內建模块
