---
title: shell
category: note, markdown
---

# shell是什么
- shell脚本第一行：`#!/bin/sh`
- shell注释符号#
- 运行shell脚本：
```
chmod +x test.sh
./test.sh
sh test.sh
```

# shell变量
- 变量的值可以是数字、字符串、文件、设备，或其他任何类型的数据
- 变量是指针
- 变量名：字母、数字、下划线。变量名不能数字开头，unix系统变量为大写字母
- 访问变量需要加$符号（类似于c语言中访问指针的值加*？）
- 只读变量：`readonly var_name`

# shell关键字
- True，False

# shell特殊变量
- 如下：
```
$0：当前脚本文件名
$n：脚本参数（n=1,2,3...），大于9的参数必须用大括号括起来，如${10}
$#：参数个数
$*：所有的参数当作一个参数："$1 $2 $3 ... $n"
$@：所有的参数当作不同的参数："$1", "$2", "$3", ... , "$n"（循环中一般用这种）
$?：上一个命令的退出值
$$：当前脚本的进程号
$!：上一个后台命令的进程号
```

# shell数组
- 如下：
```
定义并初始化数组：array_name=("hello" "world")
单独赋值数组成员：array_name[0]="bebye"
读取数组成员：echo ${array_name[0]}
读取所有数组成员：
echo ${array_name[*]}
echo ${array_name[@]}
```

# shell基本运算符
假设：变量`a=10`，变量`b=20`。以下三种方法一样：
```
echo `expr $a + $b`
echo $(expr $a + $b)
echo $(($a+$b))
```
**注意**：斜引号（Tab上面的那个键）括起来可以表示系统命令

- 算术运算符
```
+：加
-：减
*：乘
/：除
%：模
**：幂
=：赋值
==：相等比较，返回bool值
!=：不等比较：返回bool值
```
**注意**：
1. 条件式放在方括号内，方括号前后用空格隔开
2. 算数和表达式之间必须有空格

- 关系运算符
```
-eq：相等
-ne：不等
-gt：大于
-lt：小于
-ge：大于等于
-le：小于等于
```

- 布尔运算符
```
！：逻辑非
-o：逻辑或
-a：逻辑与
```

- 字符串运算符
```
=：相等
!=：不等
-z：长度是否为0，变量是否定义
-n：长度是否不为0
str：字符串是否非空，非空返回true
```

- 文件测试操作
```
-b：是否是块文件
-c：是否是字符文件
-d：是否是目录
-f：是否是普通文件
-g：是否设置gid（SGID）
-k：是否设置sticky位
-p：是否是管道文件
-t：文件描述符是否打开并关联一个终端
-u：是否设置uid（SUID）
-r：是否可读
-w：是否可写
-x：是否可执行
-s：文件大小是否大于0
-e：是否存在
```

# shell条件语句
- if...else语句
```
if [ expression 1 ]
then
    Statement 1
else
    Statement 2
fi
```

```
if [ expression 1 ]
then
    Statement 1
elif [ expression 2 ]
then
    Statement 2
else
    Statement 3
fi
```
- case...esac语句
```
case word in
    pattern1)
        Statement
        ;;
pattern2|pattern3)
        Statement
        ;;
esac
```
**注意**：匹配模式最小是一个。;;类似于c语言中的break

# shell循环类型
- while循环
```
while command
do
    Statement
done
```
- for循环
```
for var in word1 word2 ... wordN
do
    Statement
done
```
- until循环（和while相反，直到条件为**真**才退出循环）
```
until command
do
    Statement
done
```
- select循环
```
select var in word1 word2 ... wordN
do
    Statement
done
```
注意：select语句会打印一个选项菜单，Statement语句中可以与case语句搭配

# shell循环控制语句
- break
- continue
注意：两个语句后面都可以带一个整数N，代表从第N层循环中跳出
- exit

# shell函数
- 创建函数
```
function_name () {
    #list of commands
    echo "Hello World $1 $2"
    return n
}
```
- 调用函数
`function_name arg1 arg2 ... argn`

# 补充
- 变量比较时，最好带上双引号，否则容易报错
`if [ 1 == "$var_a" -a 1 == "$var_b" ]`
`if [ "name" == "$var_c" ]`
- `. filepath`可以把一个文件解析成bash语句，程序就可以使用这个文件里定义的变量了
- basename命令可以获取文件的文件名，不带路径
- diff命令可以比较文件
- 点命令（.）等同于source，这是一个内建命令
- `:> filename`可以把文件长度截短为0，相当于`cat /dev/null > filename`
- 一组圆括号括起来的命令是新开一个子shell来执行的，圆括号内的变量不能被外部访问
- `A > B`：把A输出到B，`A < B`：A从B中读取
- `A &> B`：把A的标准输出和标准错误重定向到B
- echo不带参数可以打印换行
- `cd -`可以快速跳转到之前的路径，是使用了`$OLDPWD`环境变量
- `Ctrl-B`退格，`Ctrl-F`前进，`Ctrl-H`退格并删除，`Ctrl-L`清屏
- 对于if的判断条件：未初始化的变量、空白、NULL都不成立；0、false等都成立
- 计算字符串长度的办法：
```
${#string}
`expr length $string`
`expr "$string" : '.*'`
```
- 把命令的输出结果赋值给变量
```
var=`sed -n '$=' xxx.txt`
```
- 变量定义和使用
定义变量时不用带$前缀，=两边不带空格。使用变量时带$前缀，不能带括号，否则报错。
