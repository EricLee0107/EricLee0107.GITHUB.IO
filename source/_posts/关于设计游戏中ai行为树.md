---
layout: game01-
title: 关于设计游戏中ai行为树
date: 2018-01-23 15:26:43
tags: [游戏开发,AI行为树]

---

>摘要
>

每个行为树需要要自己的状态,

- Failure
- Success
- Running
- Resting
- Error

#### 节点类型

##### 复合节点(Composite)

- Sequence(序列节点)：序列执行的节点流，如果一个节点执行失败，就返回，其余节点不在执行。所有节点做and操作
- Select(选择节点)：当子节点只要一个成功，就返回，其余节点不在执行。所有节点做or操作。


##### 修饰节点(Decorator)



##### 叶子节点(leaf)


#### 决策树与行为树

什么是行为树？

什么是决策树？







[关于行为树的基础概念](https://www.cnblogs.com/designyourdream/p/4459971.html)

---

未完待续......

---

