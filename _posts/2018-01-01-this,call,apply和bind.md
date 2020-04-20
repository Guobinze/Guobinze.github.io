---
layout:     post
title:      this,call,apply和bind
subtitle:   this,call,apply和bind
date:       2018-01-01
author:     GBZ
header-img: img/post-bg-debug.png
catalog: true
tags:
    - JavaScript
---


## 1 this指向哪儿
如果用一句话说明 this 的指向，那么即是: 谁调用它，this 就指向谁。this 永远指向最后调用它的那个对象。
```
    var name = "windowsName";
    function a() {
        var name = "Cherry";
        console.log(this.name);          // windowsName
        console.log("inner:" + this);    // inner: Window
    }
    a();
    console.log("outer:" + this)         // outer: Window
```
(注：这是一个全局的函数，很容易产生命名冲突，所以不建议这样使用。)
注意，这里我们没有使用严格模式，如果使用严格模式的话，全局对象就是 undefined，那么就会报错 Uncaught TypeError: Cannot read property 'name' of undefined。
```
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    a.fn();
```
```
    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
```
this 永远指向最后调用它的那个对象，因为最后调用 fn 的对象是 a，所以就算 a 中没有 name 这个属性，也不会继续向上一个对象寻找 this.name，而是直接输出 undefined。
```
    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }
    var f = a.fn;
    f();
```
这里你可能会有疑问，为什么不是 Cherry，这是因为虽然将 a 对象的 fn 方法赋值给变量 f 了，但是没有调用，再接着跟我念这一句话：“this 永远指向最后调用它的那个对象”，由于刚刚的 f 并没有调用，所以 fn() 最后仍然是被 window 调用的。所以 this 指向的也就是 window。
## 2 改变this指向
改变 this 的指向的几种方法：

- 使用 ES6 的箭头函数
- 在函数内部使用 that = this
- 使用 apply、call、bind
- new 实例化一个对象
### 2.1 箭头函数
箭头函数的 this 始终指向函数定义时的 this，而非执行时。箭头函数需要记着这句话：“箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 window”。
```
    var name = "windowsName";
    var a = {
        name : "Cherry",
        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            },100);
        }
    };
    a.func2()     // this.func1 is not a function
```
在不使用箭头函数的情况下，是会报错的，因为最后调用 setTimeout 的对象是 window，但是在 window 中并没有 func1 函数。
```
    var name = "windowsName";
    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },
        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }

    };
    a.func2()     // Cherry
```
### 2.2 在函数内部使用 that = this
如果不使用 ES6，先将调用这个函数的对象保存在变量 that 中，然后在函数中都使用这个 that，这样 that 就不会改变了。
```
    var name = "windowsName";
    var a = {
        name : "Cherry",
        func1: function () {
            console.log(this.name)     
        },
        func2: function () {
            var that = this;
            setTimeout( function() {
                that.func1()
            },100);
        }
    };
    a.func2()       // Cherry
```
这个例子中，在 func2 中，首先设置 var that = this;，这里的 this 是调用 func2 的对象 a，为了防止在 func2 中的 setTimeout 被 window 调用而导致的在 setTimeout 中的 this 为 window。我们将 this(指向变量 a) 赋值给一个变量 that，这样，在 func2 中我们使用 that 就是指向对象 a 了。
### 2.3 使用 apply、call、bind
apply 和 call 的区别是 call 方法接受的是若干个参数列表，而 apply 接收的是一个包含多个参数的数组。
```
var a = {
        name : "Cherry",
        func1: function () {
            console.log(this.name)
        },
        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.apply(a),100);
        }
    };
    a.func2()            // Cherry
```
bind 是创建一个新的函数，我们必须要手动去调用
```
 var a = {
        name : "Cherry",
        func1: function () {
            console.log(this.name)
        },
        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.bind(a)(),100);
        }
    };
    a.func2()            // Cherry
```
### 2.4 使用构造函数

```
// 构造函数:
function myFunction(arg1, arg2) {
    this.firstName = arg1;
    this.lastName  = arg2;
}
// This    creates a new object
var a = new myFunction("Li","Cherry");
a.lastName;                             // 返回 "Cherry"
```




	


