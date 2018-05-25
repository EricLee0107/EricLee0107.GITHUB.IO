---
title: shell学习笔记3
author: 李志伟
tags: [shell]
copyright: true
categories:
  - shell学习笔记
toc: true
date: 2018-05-25 15:36:35
---


**摘要：** 流程控制语句，以及常用判断条件

### 循环语句

#### for语句

**语法**
```
for var in list;
do
    commonds;
done
```
或者

```
for((i=0;i<10;i++)) 
{
    commonds;
}
```

#### while语句

**语法**
```
while condition
do
    commonds;
done
```
#### until语句

```
until condition
do
    commonds;
done
```

### 条件语句

#### if条件

```
if condition;
then
commonds;
fi
```

#### if...else if...else

```
if condition;
then
    commonds;
elif condition;
then
    commonds;
else
    commonds;
fi
```

小技巧：
`[ condition ] && action;` :如果condition为真，则执行action
`[ condition ] || action;` ：如果condition为假，则执行action

### 条件判断中的一些比较和检验

在条件中的`[`或`]`与操作数之间需要有一个空格，没有会编译报错。

#### 算数比较

**算数操作符**

- `-gt`:大于
- `-lt`:小于
- `-ge`：大于等于
- `-le`:小于等于
- `-eq`:等于
- `-eq`:不等于

**eg.**
`[ $var -gt 0]` 当var大于0是返回真


#### 字符串比较

**注意：**在字符串比较中最好使用双中括号

- `[[ $str1 = $str2 ]]`: 当str1等于str2时返回真
- `[[ $str1 == $str2 ]]`: 当str1等于str2时返回真
- `[[ $str1 != $str2 ]]`: 当str1不等于str2时返回镇
- `[[ $str1 > $str2 ]]`: 当str1字典序大于str2时返回真
- `[[ $str1 < $str2 ]]`: 当str1字典序小于str2时返回真
- `[[ -z $str1 ]]`: 当str1是空字符串时返回真
- `[[ -n $str1 ]]`: 当str1是非空字符串时返回真

#### 文件系统相关判断

- `[ -f $var ]`: 如果给定的变量包含正常的文件路径或文件名，则返回true
- `[ -x $var ]`: 如果给定的变量包含的文件可执行，则返回true
- `[ -d $var ]`: 如果给定的变量包含的是目录，则返回true
- `[ -e $var ]`: 如果给定的变量包含的文件存在，则返回true
- `[ -c $var ]`: 如果给定的变量包含的是一个字符设备文件的路径，则返回true
- `[ -b $var ]`: 如果给定的变量包含的是一个块设备文件的路径，则返回true
- `[ -w $var ]`: 如果给定的变量包含的文件可写，则返回true
- `[ -r $var ]`: 如果给定的变量包含的文件可读，则返回true
- `[ -L $var ]`: 如果给定的变量包含的是一个符号链接，则返回true


#### 结合多个条件


`[condition1 -a condition2]`：使用逻辑与`-a`
`[condition1 -o condition2]`：使用逻辑或`-o`

**eg.**
`[$var1 -ne 0 -a $var2 -ge 0]` 当var1不等于0且var2大于等于0，则返回真

`conditon1 && conditon2`:使用逻辑运算符与`&&`
`conditon1 || condition2`：使用逻辑运算符或`||`
** eg.**
`[[ -n $str1 ]] && [[ -z $str2]]`:当str1为飞空字符串并且str2为空字符串时返回真