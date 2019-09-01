---
layout:     post
title:      谈谈JavaScript中的闭包
subtitle:   谈谈JavaScript中的闭包
date:       2018-11-23
author:     GBZ
header-img: img/post-bg-debug.png
catalog: true
tags:
    - JavaScript
---


>技术学习记录

### innerHTML的功能
innerHTML在JS有两种功能：获取对象的内容或向获取的对象插入内容；


我们可以通过
```
document.getElementById('要被插入的对象***的id').innerHTML
``` 
来获取id为***的对象的内嵌内容。


也可以对获取到的对象插入内容，如
```
document.getElementById('要被插入的对象***的id').innerHTML='这是插入的内容';
```   

这样就能向id为***的对象插入内容。	

###区别getElementByID,getElementsByName,getElementsByTagName
以人来举例说明，人有能标识身份的身份证，有姓名，有类别(大人、小孩、老人)等。

1） ID 是一个人的身份证号码，是唯一的。所以通过getElementById获取的是指定的一个人。

2） Name 是他的名字，可以重复。所以通过getElementsByName获取名字相同的人集合。

3） TagName可看似某类，getElementsByTagName获取相同类的人集合。如获取小孩这类人，getElementsByTagName("小孩")。

### value和innerHTML的区别

1）value可以用来修改（获取）textarea和input的value属性的值或元素的内容

2）innerHTML用来修改（获取）HTML元素（如div）html格式的内容

######插入一个trick的解决方法，判断对象数组是否具有某个对象

    var a =[{id:1},{id:2},{name:'cc'}];
    var b = {id:1};
    console.log(JSON.stringify(a).indexOf(JSON.stringify(b))!=-1);

把数组和对象全转成string, 然后使用string.indexOf判断是否存在
### setTimeout和setInterval的区别

1）setTimeout只在指定时间后执行一次，一般情况下setTimeout用于延迟执行某方法或功能。


2）setInterval以指定时间为周期循环执行，一般用于刷新表单，对于一些表单的假实时指定时间刷新同步。





	


