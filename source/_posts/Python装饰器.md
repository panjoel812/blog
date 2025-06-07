---
title: Python装饰器
date: 2022-12-29 20:52:42
tags:
- python
categories: PFSの泡泡糖
cover: https://s2.loli.net/2024/07/09/KoIzrE3TWN42ky9.webp
top: false
description: python装饰器在平常的python编程中用到的还是很多的，在本篇文章中我们先来介绍一下python中最常使用的@staticmethod装饰器的使用。
mathjax: false
ai: 这篇文章介绍了Python中装饰器的概念和用法，以及如何自定义和使用装饰器。文章从以下几个方面介绍了装饰器的相关知识：如何使用@staticmethod装饰器来声明静态函数，以及它的作用和优点;如何使用Python类来实现自定义装饰器，以及如何利用__call__方法来处理传入的函数和返回值;如何使用Python函数嵌套来实现自定义装饰器，以及如何利用闭包来封装传入的函数和返回值;如何在一个函数上使用多个装饰器，以及它们的执行顺序和结果;如何给装饰器传递参数，以及如何使用三层函数嵌套来实现带参数的装饰器文章通过一些代码示例和输出结果来展示了装饰器的特点和功能，帮助读者理解和掌握Python中的装饰器。
---

python装饰器在平常的python编程中用到的还是很多的，在本篇文章中我们先来介绍一下python中最常使用的@staticmethod装饰器的使用。

之后，我们会使用两种不同的方式来创建自己的自定义python装饰器以及如何在其他地方进行调用。

**1、@staticmethod**

@staticmethod是python开发者经常用来在一个类中声明该函数是一个静态函数时使用到的装饰器，比如创建一个HelloWorld的python类，并且在其中使用该静态装饰器声明其中的函数。

```python
class HelloWorld():
    def __init__(self):
        super(HelloWorld, self).__init__()

    @staticmethod
    def print_hello_world():
        print('welcome to hello world!')
```

@staticmethod装饰器一般是对于一些公共的函数，或是工具函数之类的函数进行声明，声明之后就不会将当前python类中的初始化变量信息等传入到该函数中，可以看到print_hello_world函数并没有self作为参数变量。

接下来可以在初始化@staticmethod声明的函数所在的类HelloWorld，并且调用print_hello_world函数。

```python
hello_world = HelloWorld()
hello_world.print_hello_world()
```

会发现控制台直接打印出了welcome to hello world!这行字符串。

实际上在python中的函数上面加入装饰器只是为了在执行当前函数的逻辑之前去执行一些我们需要执行的业务功能，这样的操作我们通过自定义自己的装饰器也能够实现同样的效果。

**2、自定义装饰器**

其实，自定义装饰器的过程也比较简单，就是我们平常用到的函数或者python类的写法就能够实现。

自己实现装饰器主要有两种方式，一种是通过class类的方式来实现的，另外一种则是通过python函数嵌套的方式来实现的，下面我们先来通过第一种的方式来实现，也就是通过python类的方式来实现。

**python类实现装饰器**

用python类来实现装饰器时，必须明白一个知识点。python类中实际上默认有一个成员函数__call__，这个成员函数就是这个类被调用时的函数对象，若是需要自定义装饰器实际上就是在python类的__call__函数中来实现装饰器主要业务逻辑实现的。

```python
class print_message(object):
    def __init__(self, function_):
        self.function_ = function_

    def __call__(self):
        # TODO：这里实际上是对传入的函数返回值进行的装饰，可以理解成是一种回调。
        print('装饰器，{}'.format(self.function_()))
```

注意：在下面这行代码块中一定要注意self.function_是一个函数对象，而self.function_()是一个函数返回值得效果。

```python
print('装饰器，{}'.format(self.function_()))
```

这样，我们通过python类就已经实现了python装饰器的效果，使用一个函数来试验一下效果。

```python
@print_message
def hello_world():
    return 'hello world!'

hello_world()
```

调用使用了@print_message装饰器的函数，它会返回我们预期的一个函数结果的打印。

```python
# 装饰器，hello world!
```

**python函数嵌套实现装饰器**

上面的操作过程是通过重新定义了一个python类来实现装饰器的效果的，这里再使用函数嵌套的方式来实现。

因为，我们都知道在python中函数中再可以嵌套函数的，在函数中嵌套一个函数时上层的函数相对于子函数来说就是它的一个父级对象。

```python
def print_message2(function_):
    def message():
        print('装饰器2，{}'.format(function_()))

    return message

@print_message2
def hello_world2():
    return 'hello world!'

hello_world2()

# 装饰器2，hello world!
```

使用函数嵌套的方式同样实现了函数的装饰器的效果，那么思考一下若是有两个装饰器可以在同一个函数中使用吗？

**多个装饰器调用**

话不多说，为了证明两个装饰器能不能放到一个函数上面使用，我们直接试一下效果如何。

```python
@print_message2
@print_message
def hello_world3():
    return 'hello world!'

hello_world3()

# 装饰器，hello world!
# 装饰器2，None
```

从返回结果来看，首先是两个装饰器都是执行了，从数据结果打印的顺序来看自定义的装饰器的执行顺序应该是从距离函数最近的装饰器开始执行的，也就是从下往上的顺序挨个执行该函数上面的装饰器的。

另外,装饰器2的结果为None,这是为什么呢?

因为,第一个装饰器执行的时候,它的参数应该是hello_world函数本身，但是当第二个装饰器执行的时候第一个装饰器并没有返回结果知识做了打印，这个时候第二个装饰器接收到的参数自然就是None了。

**3、带参数的装饰器**

说实话带参数的装饰器在python中我见到的不多，不多在java中几乎只要是装饰器都是可以加参数执行的。

还是来介绍一下，算是属于扩展知识吧，既然已经看到了这里，不妨再多掌握个小技能吧，哈哈~

我们使用pytgon嵌套的函数功能来实现这个带参数的装饰器吧，个人觉得这种方便一些。

```python
def header(message):
    def decorator(function_):
        def wrapper():
            return '带参数的装饰器，参数：{0}，{1}'.format(message, function_())
        return wrapper
    return decorator

@header('Python 集中营')
def hello_world4():
    return 'hello world!'

print(hello_world4())

# 带参数的装饰器，参数：Python 集中营，hello world!
```

OK，带参数的装饰器果然生效了，给@header加上参数@header('Python 集中营')，上面装饰器直接使用三层函数的嵌套来实现的。

第一层函数参数是我们需要自定义给装饰器传入的参数，第二层则是传入的已经添加了装饰器的函数本身，到了第三层才是真正的处理装饰器自己的业务逻辑的。
