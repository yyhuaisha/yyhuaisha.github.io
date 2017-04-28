---
layout: post
title: 浅谈javascript的Array.prototype.slice.call(arguments)
category: 技术
tags: Array
keywords: Array,slice
---

###  浅谈javascript的Array.prototype.slice.call(arguments)

能将具有length属性的对象转成数组，除了IE下的节点集合（因为ie下的dom对象是以com对象的形式实现的，js对象与com对象不能进行转换）

``` python
var a={length:2,0:'first',1:'second'};
Array.prototype.slice.call(a);//  ["first", "second"]

var a={length:2};
Array.prototype.slice.call(a);//  [undefined, undefined]
```

slice有两个用法，一个是String.slice,一个是Array.slice，第一个返回的是字符串，第二个返回的是数组，这里我们看第2个。


``` python
Array.prototype.slice.call(arguments)
```

能够将arguments转成数组，那么就是arguments.toArray().slice();到这里，是不是就可以说Array.prototype.slice.call(arguments)的过程就是先将传入进来的第一个参数转为数组，再调用slice？



再看call的用法，如下例子
``` python
var a = function(){
     console.log(this);    // 'littledu'
     console.log(typeof this);      //  Object
     console.log(this instanceof String);    // true
}
a.call('littledu');
```

可以看出，call了后，就把当前函数推入所传参数的作用域中去了，不知道这样说对不对，但反正this就指向了所传进去的对象就肯定的了。
到这里，基本就差不多了，我们可以大胆猜一下slice的内部实现，如下:
``` python
Array.prototype.slice = function(start,end){
     var result = new Array();
     start = start || 0;
     end = end || this.length; //this指向调用的对象，当用了call后，能够改变this的指向，也就是指向传进来的对象，这是关键
     for(var i = start; i < end; i++){
          result.push(this[i]);
     }
     return result;
}
```

最后，附个转成数组的通用函数:

``` python
var toArray = function(s){
    try{
        return Array.prototype.slice.call(s);
    } catch(e){
            var arr = [];
            for(var i = 0,len = s.length; i < len; i++){
                //arr.push(s[i]);
                   arr[i] = s[i];  //据说这样比push快
            }
             return arr;
    }
}
```