PYTHON 学习笔记

[TOC]

# python 基础操作

## python 列表生成器

List Comprehensions

```python
>>> ['a' for i in range(4) if i % 2==0]
['a', 'a']
```

### 行列转换

```python
[[row[col] for row in matrix] for col in range(len(matrix[0]))]
```



## python 文件读取

打开一个文件

```python
file=open(filename, 'w')
data=file.read()
data=file.readline()
data=file.readlines() #生成一个列表	
file.close()
```

文件读写有可能生成`IOError`，后面的`f.close()`有可能不会调用，所以会采用`try...finally:`，不过更好的办法是

```python
with open('/path/file', 'r') as f:
    print(f.read())
```

file like object

```python
f=open('/path/*.jpg', 'rb') #二进制 b binary mode
f=open('/path/file', 'r', 'enconding='gbk') #其他编码类型
```

r 以读的方式打开
w 以写方式打开
a 以追加模式打开 (从 EOF 开始, 必要时创建新文件)
r+ 以读写模式打开( 从头开始写，如果先读，`f.readline()`，则从末位开始写)
w+ 以读写模式打开 (默认清空，如果要读的话，需要`seek()`移动指针)
a+ 以读写模式打开 (如需读取，需要`seek()`指针 )

# metaclass

## 类也是对象

当使用关键词__class__,python会生成一个ObjectCreator的对象

``` python
class ObjectCreator(object):
    pass
```

当一个__对象__具有创建对象的能力，称之为__类__。

##type 创建类

~~~python
type('Foochild',(foo),{'bar':True}
~~~

等价于

```python
class FooChild(foo):
	bar=True
```



## 什么是元类

定义类创建对象，那通过什么创建类？

__元类__

我们已经知道**type**可以创建类：

```python
Myclass=type('MyClass',(),{})
```

**type**函数实际是一个元类

通过检查**class**属性，我们发现python中任何的数据类型都是对象，包括整数、字符串、函数以及类。

```python
>>> age=35
>>> age.__class__
<class 'int'>
```

那么`__class__`的`__class__`是什么？

```python
>>> age.__class__.__class__
<class 'type'>
```

所有类通过元类来创建，**type**是内置的元类，python默认使用它类创建类。
当然，我们也可以定义自己的类。

## metaclass属性

metaclass的使用方式

```python
class Foo(metaclass=Metaclass):
    pass
```



metaclass的定义

```python
class Metaclass(type):
    def __new__(cls, clsname, bases, attrs):
        #do sth
        return type.__new__(cls, clsname, bases, attrs)
```

更好的是通过**super**方法

```python
class Metaclass(type):
    def __new__(cls, clsname, bases, attrs):
        #do sth
        return super(Metaclass, cls).(cls, clsname, bases, attrs)
       #super(子类，某个class的mro).父类的方法
```

