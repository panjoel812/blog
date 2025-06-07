---
title: Python 正则表达式一文通
date: 2022-12-27 08:29:26
tags:
- python
categories: PFSの泡泡糖
cover: https://s2.loli.net/2024/07/09/sIWF8Hc1GRDJp4j.webp
top: false
description: 正则表达式用于识别文本字符串中的搜索模式，它还有助于找出数据的正确性，甚至可以使用正则表达式进行查找、替换和格式化数据等操作。
businesscard: false
mathjax: false
ai: 这篇文章介绍了正则表达式的概念和用途，以及一些常见的正则表达式操作的示例。正则表达式是一种用于识别和处理文本字符串中的搜索模式的工具，它可以用于验证数据的正确性、查找和替换数据、格式化数据等。文章通过一些场景来展示了正则表达式可以解决的问题，例如从日志文件中提取日期和时间、从电子邮件地址中过滤掉无效的地址、验证和分类电话号码、更新学生数据库中的地区代码等。文章还演示了如何使用正则表达式来匹配单词、生成迭代器、匹配一系列字符范围、替换字符串、解决反斜杠问题、匹配单个字符等。
---

## **为什么要使用正则表达式**

为了回答这个问题，我们先来看看我们面临的各种问题，而这些问题又可以通过使用正则表达式来解决。

考虑以下场景：

文末有一个包含大量数据的日志文件，从这个日志文件中，希望只获取日期和时间。乍一看，日志文件的可读性是很低的。

![日志文件](https://s1.ax1x.com/2022/12/27/zzpCnJ.png)

在这种情况下，可以使用正则表达式来识别模式并轻松提取所需信息。

考虑下一个场景：你是一名销售人员，有很多电子邮件地址，其中很多地址都是假的/无效的，看看下面的图片：

![无效地址](https://s1.ax1x.com/2022/12/27/zzpPB9.png)

我们可以做的是使用正则表达式，可以验证电子邮件地址的格式并从真实 ID 中过滤掉虚假 ID。

下一个场景与销售员示例的场景非常相似，考虑下图：

![虚假ID](https://s1.ax1x.com/2022/12/27/zzpi7R.png)

我们如何验证电话号码，然后根据原产国对其进行分类？

每个正确的数字都会有一个特定的模式，可以通过使用正则表达式来跟踪和跟踪。

接下来是另一个简单的场景：

我们有一个学生数据库，其中包含姓名、年龄和地址等详细信息。考虑一下地区代码最初是 59006 但现在已更改为 59076 的情况，这种情况为每个学生手动更新此代码将非常耗时且过程非常漫长。

基本上，为了使用正则表达式解决这些问题，我们首先从包含 pin 码的学生数据中找到一个特定的字符串，然后将它们全部替换为新字符串。

## **什么是正则表达式**

正则表达式用于识别文本字符串中的搜索模式，它还有助于找出数据的正确性，甚至可以使用正则表达式进行查找、替换和格式化数据等操作。

考虑以下示例：

![个人信息表格](https://s1.ax1x.com/2022/12/27/zzpkA1.png)

在给定字符串的所有数据中，假设我们只需要城市，这可以以格式化的方式转换为仅包含名称和城市的字典。现在的问题是，我们能否确定一种模式来猜测名称和城市？此外我们也可以找出年龄，随着年龄的增长，这很容易，对吧？它只是一个整数。

我们如何处理这个名字？如果你看一下这个模式，所有的名字都以大写字母开头。借助正则表达式，我们可以使用此方法识别姓名和年龄。

我们可以使用下面的代码

```Python
import re
 
Nameage = '''
Janice is 22 and Theon is 33
Gabriel is 44 and Joey is 21
'''
 
ages = re.findall(r'd{1,3}', Nameage)
names = re.findall(r'[A-Z][a-z]*',Nameage)
 
ageDict = {}
x = 0
for eachname in names
    ageDict[eachname] = ages[x]
    x+=1
print(ageDict)
```

Output:

```python
{'Janice': '22', 'Theon': '33', 'Gabriel': '44', 'Joey': '21'}
```

## **正则表达式几个示例：**

可以使用正则表达式执行许多操作。在这里，我列出了一些帮助更好地理解正则表达式的用法非常重要的内容。

让我们首先检查如何在字符串中找到特定单词

### **在字符串中查找一个单词**

```python
import re
 
if re.search("inform","we need to inform him with the latest information"):
    print("There is inform")
```

我们在这里所做的一切都是为了搜索单词 inform 是否存在于我们的搜索字符串中。

当然我们还可以优化以下代码

```python
import re
 
allinform = re.findall("inform","We need to inform him with the latest information!")
 
for i in allinform:
    print(i)
```

在这里，在这种特殊情况下，将找到两次`infor`。一个来自`inform`，另一个来自`information`。

如上所示，在正则表达式中查找单词就这么简单。

接下来我们将了解如何使用正则表达式生成迭代器。

### **生成迭代器**

生成迭代器是找出并目标字符串的开始和结束索引的简单过程。考虑以下示例：

```python
import re
 
Str = "we need to inform him with the latest information"
 
for i in re.finditer("inform.", Str):
    locTuple = i.span()
    print(locTuple)
```

对于找到的每个匹配项，都会打印开始和结束索引。当我们执行上述程序时，输出如下：

```python
(11, 18)
(38, 45)
```

接下来我们将检查如何使用正则表达式将单词与模式匹配。

### **将单词与模式匹配**

考虑一个输入字符串，我们必须将某些单词与该字符串匹配。要详细说明，请查看以下示例代码：

```python
import re
 
Str = "Sat, hat, mat, pat"
 
allStr = re.findall("[shmp]at", Str)
 
for i in allStr:
    print(i)
```

字符串中有什么共同点？可以看到字母“a”和“t”在所有输入字符串中都很常见。代码中的 [shmp] 表示要查找的单词的首字母，因此，任何以字母 s、h、m 或 p 开头的子字符串都将被视为匹配，其中任何一个，并且最后必须跟在“at”后面。

Output:

```python
hat
mat
pat
```

接下来我们将检查如何使用正则表达式一次匹配一系列字符。

### **匹配一系列字符范围**

我们希望输出第一个字母应该在 h 和 m 之间并且必须紧跟 at 的所有单词。看看下面的例子，我们应该得到的输出是 hat 和 mat

```python
import re
 
Str = "sat, hat, mat, pat"
 
someStr = re.findall("[h-m]at", Str)
 
for i in someStr:
    print(i)
```

Output:

```python
hat
mat
```

现在让我们稍微改变一下上面的程序以获得一个不同的结果

```python
import re
 
Str = "sat, hat, mat, pat"
 
someStr = re.findall("[^h-m]at", Str)
 
for i in someStr:
    print(i)
```

发现细微差别了吗，我们在正则表达式中添加了插入符号 (^)，它的作用否定了它所遵循的任何效果。我们不会给出从 h 到 m 开始的所有内容的输出，而是会向我们展示除此之外的所有内容的输出。

我们可以预期的输出是不以 h 和 m 之间的字母开头但最后仍然紧随其后的单词。Output:

```python
sat
pat
```

### **替换字符串：**

接下来，我们可以使用正则表达式检查另一个操作，其中我们将字符串中的一项替换为其他内容：

```python
import re
 
Food = "hat rat mat pat"
 
regex = re.compile("[r]at")
 
Food = regex.sub("food", Food)
 
print(Food)
```

在上面的示例中，单词 rat 被替换为单词 food。正则表达式的替代方法就是利用这种情况，它也有各种各样的实际用例。Output:

```python
hat food mat pat
```

### **反斜杠问题**

```python
import re
 
randstr = "Here is Edureka"
 
print(randstr)
```

Output:

```python
Here is Edureka
```

这就是反斜杠问题，其中一个斜线从输出中消失了，这个特殊问题可以使用正则表达式来解决。

```python
import re
 
randstr = "Here is Edureka"
 
print(re.search(r"Edureka", randstr))
```

Output:

```python
<re.Match object; span=(8, 16), match='Edureka'>
```

这就是使用正则表达式解决反斜杠问题的简单方法。

### **匹配单个字符**

使用正则表达式可以轻松地单独匹配字符串中的单个字符

```python
import re
 
randstr = "12345"
 
print("Matches: ", len(re.findall("d{5}", randstr)))
```

Output:

```python
Matches: 1
```

### **删除换行符**

我们可以在 Python 中使用正则表达式轻松删除换行符

```python
import re
 
randstr = '''
You Never
Walk Alone
Liverpool FC
'''
 
print(randstr)
 
regex = re.compile("
")
 
randstr = regex.sub(" ", randstr)
 
print(randstr)
```

Output:

```python
You Never
Walk Alone
Liverpool FC

You Never Walk Alone Liverpool FC
```

可以从上面的输出中看到，新行已被空格替换，并且输出打印在一行上。

还可以使用许多其他东西，具体取决于要替换字符串的内容

```python
: Backspace
: Formfeed

: Carriage Return
: Tab
: Vertical Tab
```

可以使用如下代码

```python
import re
 
randstr = "12345"
 
print("Matches:", len(re.findall("d", randstr)))
```

Output:

```python
Matches: 5
```

从上面的输出可以看出，d 匹配字符串中存在的整数。但是，如果我们用 D 替换它，它将匹配除整数之外的所有内容，与 d 完全相反。

接下来我们了解一些在 Python 中使用正则表达式的重要实际例子。

## **正则表达式的实际例子**

我们将检查使用最为广泛的 3 个主要用例

- 电话号码验证
- 电子邮件地址验证
- 网页抓取

### **电话号码验证**

需要在任何相关场景中轻松验证电话号码

考虑以下电话号码：

- 444-122-1234
- 123-122-78999
- 111-123-23
- 67-7890-2019

电话号码的一般格式如下：

- 以 3 位数字和“-”符号开头
- 3 个中间数字和“-”号
- 最后4位数

我们将在下面的示例中使用 w，请注意 w = [a-zA-Z0-9_]

```python
import re
 
phn = "412-555-1212"
 
if re.search("w{3}-w{3}-w{4}", phn):
    print("Valid phone number")
```

Output:

```python
Valid phone number
```

### **电子邮件验证**

在任何情况下验证电子邮件地址的有效性。

考虑以下电子邮件地址示例：

- Anirudh@gmail.com
- Anirudh@com
- AC.com
- 123 @.com

我们只需一眼就可以从无效的邮件 ID 中识别出有效的邮件 ID，但是当我们的程序为我们做这件事时，却并没有那么容易，但是使用正则，就非常简单了。

指导思路，所有电子邮件地址应包括：

- 1 到 20 个小写和/或大写字母、数字以及 . _ % +
- 一个@符号
- 2 到 20 个小写和大写字母、数字和加号
- 一个点号
- 2 到 3 个小写和大写字母

```python
import re
 
email = "ac@aol.com md@.com @seo.com dc@.com"
 
print("Email Matches: ", len(re.findall("[w._%+-]{1,20}@[w.-]{2,20}.[A-Za-z]{2,3}", email)))
```

Output:

```python
Email Matches: 1
```

从上面的输出可以看出，我们输入的 4 封电子邮件中有一封有效的邮件。

这基本上证明了使用正则表达式并实际使用它们是多么简单和高效。

### **网页抓取**

从网站上删除所有电话号码以满足需求。

要了解网络抓取，请查看下图：

![网络抓取](https://s1.ax1x.com/2022/12/27/zzpe1O.png)

我们已经知道，一个网站将由多个网页组成，我们需要从这些页面中抓取一些信息。

网页抓取主要用于从网站中提取信息，可以将提取的信息以 XML、CSV 甚至 MySQL 数据库的形式保存，这可以通过使用 Python 正则表达式轻松实现。

```python
import urllib.request
from re import findall
 
url = "http://www.summet.com/dmsi/html/codesamples/addresses.html"
 
response = urllib.request.urlopen(url)
 
html = response.read()
 
htmlStr = html.decode()
 
pdata = findall("(d{3}) d{3}-d{4}", htmlStr)
 
for item in pdata:
    print(item)
```

Output:

```python
(257) 563-7401
(372) 587-2335
(786) 713-8616
(793) 151-6230
(492) 709-6392
(654) 393-5734
(404) 960-3807
(314) 244-6306
(947) 278-5929
(684) 579-1879
(389) 737-2852
...
```

我们首先是通过导入执行网络抓取所需的包，最终结果包括作为使用正则表达式完成网络抓取的结果而提取的电话号码。
