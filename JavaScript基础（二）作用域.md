---
title: JavaScript基础（二）作用域
tags:
  - 前端
  - JavaScript基础
categories: JavaScript基础
abbrlink: 58985
date: 2019-01-14 21:37:43
---
## 一、何为作用域？
- 如我们所知的，JavaScript 的函数是一个对象，那么既然是一个对象，就会有属性，然而对象中有一些属性我们可以访问有一些却是我们访问不了的，访问不了的那些是JavaScript引擎存取的，那么我们的作用域[[scope]]就是其中一个。作用域[[scope]]存储了运行期上下文的集合。
<!--more-->
- 那么问题又来了，执行期上下文又是什么呢，就是当函数执行时，会创建一个称为执行期上下文的内部对象，一个执行上下文定义了一个函数执行时的环境，函数每次执行时对应的执行上下文是独一无二的，并且当函数执行结束时，它所产生的执行期上下文会被销毁。
- 接着说到作用域，那也要说一下**作用域链**，作用域[[scope]]中存储的执行期上下文对象的集合呈链式链接，所以叫作用域链。
- 那么我们查找变量就是从函数作用域链的顶端依次向下查找
## 二、结合例子讲解

```JavaScript
function a () {
    var aa = 2;
    function b () {
        var aa = 3;
        function c () {

        }
        c();
    }
    b();
    console.log(aa);
}

a();

```
1. 首先 a函数被 defined，所以自然就产生了它的属性[[scope]],[[scope]]里面存的是作用域链，作用域链自然就是对象的集合，所以此时a在被定义时的作用域链就是此时的所处的执行期上下文，那么这个环境就是全局对象，因此我们的作用域链中仅有一位[0]位-- global object（下面简称GO）【因为函数a刚在全局中被定义，那它的执行期上下文自然只有一个全局的对象】具体可看图一：{%asset_img 1.jpg %}

2. 接着就是a函数执行，a函数执行，一个函数执行必然产生一个函数独一无二的执行期上下文即产生属于a的AO（acitive object），那么这个AO就是放在作用域链的最顶端（任何环境的作用域链的最顶端一定是该环境下的AO）。所以把AO放在顶端【0】位，那么原来的在第一位的GO就放在第二位，作用域链如图二所示：

3. 在a函数执行的中，b被定义，因此b的作用域就是此时所处的环境的执行上下文，即函数a的作用域链，即图二所示；{%asset_img 2.jpg %}
4. 接着b执行，产生自己的执行期上下文，b的AO放在作用域的最顶端，所以此刻b的作用域链即首位[0]--b的AO+b被定义时所处的环境，如图三所示：{%asset_img 3.jpg %}
5. 再接着c被定义(同理)，c被执行（也同理）
6. 最后打印出来的aa的值为3；因为b函数执行的作用域链中a的AO跟原本a函数执行的AO是同一个；
7. 当函数执行结束时，其执行期上下文会被立即销毁，例如b执行完后，那么b的AO会被销毁，但由于a函数还没执行结束，所以a的AO还继续存在。
8. 当再次调用函数a时，将产生新的作用域链，与之前的是否调用执行没有关系。