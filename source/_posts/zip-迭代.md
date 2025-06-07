---
title: zip()迭代
date: 2022-05-28 18:25:26
tags:
- python
categories: PFSの泡泡糖
cover: https://tse4-mm.cn.bing.net/th/id/OIP-C.k_4k4tgWR2R_sNjbZ-pvqwHaEK?w=281&h=180&c=7&r=0&o=5&pid=1.7
top: false
ai: 这篇文章介绍了如何使用Python的zip()函数来实现并行迭代的功能。文章通过一个简单的例子来展示了zip()函数的用法，即将三个元组按照对应位置的元素打包成一个新的元组，并用for循环来遍历这个新的元组，从而实现了同时迭代三个序列的效果。文章还说明了zip()函数会在最短的序列结束时停止迭代，因此如果序列长度不一致，会有一些元素被忽略。
---

## 使用zip()并行迭代

```python
names = ("pye", "pyx", "gzm", "zbr")
ages = (13, 15, 13, 13)
jobs = ("Students", "Students", "Students")
for name, age, job in zip(names, ages, jobs):
	print("{0}--{1}--{2}".format(name, age, job))
```

结果：

```python
>>> pye--13--Students
pyx--15--Students
gzm--13--Students
```
