[TOC]

# javascript基础

## strict

启用strict模式

`'use strtrict';`

强制通过var申明变量

## 字符串

多行字符

```javascript
`这是一个
多行
字符串`；
```

模板字符串

```javascript
var name='小明'
var message=`你好，${name}`
```



字符串可以通过索引访问

```javascript
var s='hello'
s[0]; // 'H'
s[0]='X' // 改变它没有任何意义
```

字符串方法

```javascript
s.toUpperCase()
s.toLowerCase()
s.indexOf('sth') //返回sth的起始位置的索引
s.subsrting(0,5) //返回0-4的字符，第一位可省略即s.substring(5)
```



## 数组

基础操作

```javascript
var arr=[1,2,3,4,5]
arr.length; //5

arr.length=6； //给长度赋值会改变数组，产生undefined
arr[1]=99; //访问索引可以改变值
arr[5]='x' //访问不存在的索引会改变arr的大小
```



基本方法

```javascript
arr.indexOf(sth)//sth的索引值
arr.slice(0,3)// 切片器，0可省略
arr.push('sth') //向末尾增加'sth'
arr.pop() //删除最后一个元素
arr.unshift('sth') //向开头增加'sth'
arr.shift() // 删除第一个元素
arr.sort() //排序
arr.sort(function) //通过function排序
arr.reverse()//反转
arr.splice(2,3,'google','facebook')//从索引2开始删除3个元素，并添加两个元素
arr.concat(arr2) //凭借arr arr2
arr.join('-')
```

## 对象

```javascript
var xiaoming={
    name: '小明',
    birth: 1990,
    school:'No.1 Middle School'
    'middle-school':'No.2 Middle School' //middle-school包含特殊字符，必须用''括起来，且不能通过.访问
}
xiaoming.name; //'小明' 或者xiaoming['name']
xiaoming['middle-school'] //只能通过此方式
```

*javaScript对象的属性都是字符串*

基本方法

```javascript
delete.xiaoming.age //删除age属性
'name' in xiaoming //判断是否有name属性,包括继承的属性
xiaoming.hasOwnProperty('name')//判断是否是自身拥有的属性
```



## 条件判断

```javascript
if(true){

}else{

} //单个条件，不要省略{}以免产生不可预见的后果

if(sth is true){
    
}else if(sth is true){
    
}else{
    
}//其实是两层if...else 但因为else只包含了if语句，所以省略{}增加可读性


```



### 语法糖

`x>y ? a : b` 如果true就返回a，反之b



## 循环

**for循环**

```javascript
var i;
for (i=1;i<10000;i++){ // for(::)无限循环
    do sth;
}//i++在循环后执行
```



**for in 循环**

```javascript
for (var key in o){
    if (o.hasOwnProperty(key)){  //过滤掉继承的对象
    do sth;
    }
}
```



**for of 循环**

可以避免额外的对象,必须是iterable对象，Array、Map、Set

更好的办法,iterable使用内置方法

```javascript
'use strict';
var a=['A','B','C'];
a.forEach(function(element,index,arry)){  //element指向当前元素，index指向索引，arr指向数组
          console.log(element+', index=' +index);
          }
```

**while**

```javascript
while(sth){
    do sth;
}
do{
    do sth;
}while(sth)
```



## map和set

**map**

为了解决对象额属性都是字符串的问题

```javascript
var m=new Map([['Michael',95],['bob',75]])//初始化必须是一个二元数组,或者空值
m.get('Michael')
m.set('Adam',67)
m.has('Adam')
m.delete('Adam')
```

**set**

```
var s =new Set([1,2,3,4,5])//重复元素会被删除，可以建立一个空数组
s.add(sth)
s.delete(sth)

```

如果使用内置forEach函数

```javascript
var s=new Set(['A','B','C']);
s.forEach(function(element,sameElement,set)){  //前面两个参数都是element，因为没有索引
          do sth;
          }
```

# 函数

## 函数定义

```javascript
function abs(x){
    return sth;
}
```

允许传入任意个参数，包括0

**arguments**

通过arguments可以拿到调用者传入的参数

```javascript
arguments.length //获取参数个数
function foo(a,b,...rest){ //通过...rest获取多余的参数
    return sth;
}
```

**return**

```javascript
function foo{
return
{name:'foo'};
}
将返回undefined
```

## 变量申明

一个好习惯

```javascript
function foo(){
    var
        x=1,
        y=2,
        z=3
    sth else;
}
```

全局变量会作用到window上

另一个好的习惯

```javascript
var MYAPP={};
MYAPP.name='myapp'
MYAPP.version=1.0
MYAPP.foo=function(){
    return sth;
}
```

**let**

变量的作用域是函数内部
通过let申明一个会计作用域的变量

```javascript
for (let i=0;i<100;i++){
    do sth;
}
```

**const**

`const PI=3.14;`

## this apply call

`this.age` 在对象内部用this返回找个对象

找个this会有很多问题

如果在某个方法内部

可以事先指明 

`var  that=this`

或者

`function.appy(sth，[])`  this 指向sth []代表参数
`function.call(sth,a,b,c) `this 指向sth,a,b,c代表参数