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

# python 虚拟环境

```shell
$mkdir ~/.venvs
$virtualenv --system-site-packages ~/.venvs/lpthw 
$. ~/venvs/lpthw/bin/activate
$(lpthw)
```

首先创建一个文件夹储存所有的虚拟环境
执行`virtualenv`让它包括系统站点，并在lpthw中创建一个虚拟环境
最后，通过souce命令激活

# python项目骨架

通常包含 bin NAME tests docs 四个文件夹

```shell
.
├── NAME
│   └── __init__.py
├── bin
├── docs
├── setup.py
└── tests
    └── __init__.py
```

# python 爬虫

## requests

基本操作

```python
import requests
r=requests.get(url)
r=requests.post(url,data=data)
```

传递参数，定制响应头

```python
r=requests.get(url, headers=headers,params=params, cookies=cookies)
```

获取响应内容

```
r.text
r.url
r.status_code
r.json()
```

## json操作

通过json.dumps将字典转换成json
通过json.loads将json转换成字典

```python
>>> import json
>>> d=dict(name='bob', age=20)
>>> json.dumps(d)
'{"name": "bob", "age": 20}'
>>> json_str='{"age":false, "key":null, "name":"bob" }'
>>> json.loads(json_str)
{'age': False, 'key': None, 'name': 'bob'}
>>> a=json.loads(json_str) #获取字典里的内容办法
>>> a.get('age')
False
```

# numpy

## dataframe基本操作

### 新建

pd.DataFrame

```python
import pandas as pd
import numpy as np
>>> d={'col1':['name1',2],'col2':['name2',4]}
>>> df=pd.DataFrame(data=d)
>>> df
    col1   col2
0  name1  name2
1      2      4

>>> df2 = pd.DataFrame(np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]]),
... columns=['a', 'b', 'c'])
>>> df2
   a  b  c
0  1  2  3
1  4  5  6
2  7  8  9
```

### 查看类型

```python
df.head(3) ##查看前三行
df.tail(3) ##查看尾三行
df.index 
df.colunms
df.dtype
df.describe()
```

### 转置,排列

```
df.sort_index(axis=0,ascending=False) # 通过行列标签来排列 axis∈{0，1}
df.sort_value(by='columnname') #选择某一列进行排列
df.T 转置
```

### 选择

```python
df['A'] #选择A 列
df['a':'a'] #选择a行
df[0：3] #选择前三行 
```

通过标签选择

```python
df.loc['a'] #选择a行
df.loc['a','A'] #选择a行A列
df.loc[:,'A'] #选择A列
```

通过数字定位

```python
df.iloc[1,2] #第一行，第2个
```

筛选数据

```python
df[df.A>0]  #选择 A列大于0的 行
df.loc[df.A>0, 'A'] #选择 A列大于0的 数
df.loc[lambda df:df.A>0, :]
df.loc[: lambda df: ['A','B']] #选择 A，B列
```

### 操作数据

```python
df.mean(0) #计算平均值 0,1改变行列
df.sub(s, axies='index') 
df.apply(pd.to_numeric)
df.apply(pd.to_numeric, errors='ignore')
#更好的办法
df.infer_objects()
```

### 分组

```python
df.groupby('A').sum()
df.groupby(['A','B']).sum()
```

## df.to_excel

基本方式

```python
writer=pd.ExcelWriter('pandas_sample.xlsx', engine='openpyxl')
df.to_excel(writer,sheet_name='sheet_name')
writer.save()
```

追加数据

```python
from openpyxl import load_workbook
excelpath='excel_path'
with pd.ExcelWriter(excelpath, engine='openpyxl') as writer:
    writer.book=load_workbook(excelpath)
    df.to_excel(excel_writer=writer,sheet_name='sheet_name')
    
```

使用openpyxl直接写入

```python
from openpyxl import load_workbook
from openpyxl.utils.dataframe import dataframe_to_rows
excelpath='excel_path.xlsx'
wb=load_workbook(excelpath) :
ws=wb['sheetname']
for r in dateframe_to_rows(df, index=True, header=True):
    ws.append(r)
wb.save(excelpath)

```



# openpyxl

## 基本操作

新建一个excel

```python
from openpyxl import Workbook
wb=Workbook()
ws=wb.active() #获取当前活动的sheet
ws.title='sheet_name'
for row in rows:
    ws.append(sth)
ws2=wb.create_sheet('sheet_name2')
wb.save('filename')
```



载入一个excel

```python
from openpyxl import load_workbook
excelpaht=sth
wb=load_workbook(excelpath)
ws=wb['sheet_name']
wb.save(excelpath)
```

其他基本方法

```python
wb.creat_sheet(title=None, index=None)
wb.remove_sheet(ws) #ws是worksheet对象，不是worksheet的名字
wb['sheet'] #获取某个sheet
wb.sheetnames #返回sheetname list
ws['D18'].value #获取某个单元格的值
```

see more https://openpyxl.readthedocs.io