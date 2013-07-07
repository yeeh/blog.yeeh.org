---
layout: post
title: 学学jquery源码
tag: jquery
category: 手
---
那天面试还有一题，同样没实现出来。大概如下：
var Do;
//代码开始
//-----代码部分
//代码结束
var a = Do("aa");
a.bt("bt");
补全代码，实现a.bt输出结果为aabt。

如果是var a = new Do("aa")，那么直接定义Do为函数，给Do添加方法即可。可是没有new，那么就需要自己构造增强函数了。之前看过点jquery源码，但是没留意这个=_=。今天把jq的翻出来复看，发现了这个秘密...

var Do;
//代码开始
Do = function(a) {
     return new Do.fn.init(a);
};
Do.fn = Do.prototype = {
     temp: "",
     init: function(a) {
            this.temp = a;
            return this;
     },
     bt: function(b) { document.write(this.temp + b ) }
}
Do.fn.init.prototype = Do.fn;
//代码结束
var a = Do("aa");
a.bt("bt");

jq对应的是
// Give the init function the jQuery prototype for later instantiation
jQuery.fn.init.prototype = jQuery.fn;