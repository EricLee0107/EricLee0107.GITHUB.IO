---
title: leetcode-9PalindromeNumer
copyright: true
toc: true
date: 2019-05-26 10:59:07
tags: leetcode
---
#### 题目摘要

判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

<!--more-->
#### 思路

一些一定不为回文数的数：
1. 负数
2. 大于0，但末位为0的数（x> 0 && x % 10 == 0）

如果是上面这些情况则直接返回false。

将整数的每一位存入数组中，arr[i]代表整数的倒数i+1位。

然后判断arr[i]是否和arr[num -1 -i]位是否相等，如果相等则判断下一位；否则返回false
当i> num-i-1时如果还没返回则代表所有字符的是相对称的，也就是回文数，返回true

#### 代码
```c++
bool isPalindrome(int x) 
{
	if (x < 0) return false;
	if (x > 0 && x % 10 == 0) return false;
	int arr[100];
	int num = 0;
	while (x != 0)
	{
		arr[num] = x % 10;
		x = x / 10;
		num++;
	}
	int i = 0;
	while (i <= num - 1 - i)
	{
		if (arr[i] == arr[num - 1 - i])
			i++;
		else
			return false;
	}
	return true;
}

```