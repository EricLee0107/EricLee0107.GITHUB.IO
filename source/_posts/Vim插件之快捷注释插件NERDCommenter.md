---
title: Vim插件之快捷注释插件NERDCommenter
date: 2018-02-05 14:58:03
tags: [Vim]

---
**常用的**
- `[count]<leader>cc` 注释N行或注释掉选中行
- `[count]<leader>cu` 取消注释(与cc相对)
- `[count]<leader>c<space>` 如果选中的第一行已经被注释则所有选中行都变为非注释,如果第一行为非注释，则再次注释所有内容。
- `[count]<leader>ci` 切换选中行的注释状态(已经注释的变为未注释，未注释的变为注释)
- `<leader>c$` 注释当前行光标所在位置及之后的内容
- `<leader>cA` 在行末添加注释分隔符并进入插入模式(一般给当前行写说明时会用)

**不常用的**

- `[count]<leader>cn` 
- `[count]<leader>cm`
- `[count]<leader>cs`
- `[count]<leader>cy` 基本等于cc
- `<leader>ca` 切换分隔符
- `[count]<leader>cl` 基本等于cc
- `[count]<leader>cb` 基本等于cc
