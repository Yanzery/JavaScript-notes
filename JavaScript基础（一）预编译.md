---
title: JavaScript基础（一）预编译
date: 2019-01-13 19:51:00
tags: JavaScript基础
categories: web前端
---

# JavaScript（一）预编译
## 1. 关于JavaScript
众所周知 JavaScript 是一门脚本语言，为了缩短传统的“编写、编译、链接、运行”过程而创建的计算机编程语言。一个脚本是通常是解释运行而非编译，因此 JavaScript 也是一门解释性语言，即解释一行执行一行。
### JavaScript执行的三部曲
- 语法分析
- 预编译
- 解释执行
<!--more-->
> 语法分析：即先通篇检查有没有语法错误，若出现语法错误，即该模块无法执行；


> 解释执行：即执行代码；


> **预编译：** 可以理解为在函数执行前在内存空间开辟一些空间，存放一些变量和函数；理解了预编译能够加深对作用域的理解；

## 2. 预编译
### 2.1 预编译的前奏
1. imply global 暗示全局变量：即任何变量，如果变量未经声明，就赋值，此变量就为全局对象所有。
```
a = 123；
var a = b = 123； // b未经声明就被赋值为123；  
```
2. 一切声明的全局变量，全是 window 的属性；
3. window就是全局的域

### 2.2 预编译的四个步骤
1. 创建 AO （Activation Object 即执行期上下文） 对象；
2. 找形参和变量声明，将变量和形参名作为 AO 属性名，值为 undefined；
3. 将实参值和形参统一；
4. 在函数体里面找函数声明，值赋予函数体；
#### 例如：

```JavaScript
function fn (a) {
    console.log(a);//a1
     var a = 123;

     console.log(a);//a2

     function a () {}//函数声明
     console.log(a)//a3

     var b = function () {}//函数表达式

     console.log(b)

     function d () {}
    }
    fn(1)

```
#### 解析如下：
##### 1. 预编译过程：
1. 创建 AO 对象
2. 找形参和变量声明，将变量和形参名作为 AO 属性名，
值为 undefined；

```
AO{
    a : undefined，
    b : undefined，
}
```

3. 将实参和形参相统一

```
AO{
    a : 1,
    b : undefined，
}
```

4. 在函数体里面找函数声明，值赋予函数体；

```
AO{
    a : function a () {},
    b : undefined,
    d : function d () {},
}
```

5. 执行函数

##### 2. 执行函数
1. 首先打印a，在AO对象中寻找a，所以a1为function a () {}
2. 执行 var a = 123；不完全执行，因为在步骤二中找变量声明，
已经把变量提升了，但是a = 123；还没读，所以继续在 AO 对象
中寻找属性名 a ，并执行 a = 123，将 a 的值 赋值为123；
**AO**
变为：

```
AO{
    a : 123,
    ....
}
```

因此打印出来的 a，即 a2 为 123；
3. 函数声明已经变量提升了；a3为123；
4. 执行 
```
b = function () {}
```
,所以 b 为function () {}