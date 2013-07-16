---
layout: post
title: 算页数
date: 2006-03-08 17:10:46
categories: 手
---
RecordCount=101
PageSize=7

以vbs为例,怎么算页数i?

1.
{% highlight javascript %}
If RecordCount mod PageSize =0 Then
 i=RecordCountPageSize
Else
 i=RecordCountPageSize+1
End If
{% endhighlight %}

2.
{% highlight javascript %}
i=Abs(Int(-Abs(RecordCount/PageSize)))
{% endhighlight %}
3.
{% highlight javascript %}
i=-Int(-RecordCount/PageSize)
{% endhighlight %}
4.
{% highlight javascript %}
i=(RecordCount+PageSize-1)PageSize
{% endhighlight %}
用1最多.其次是用2的多,辅以少量用3的,最少是用4的.

顺便记录js的.

1.
{% highlight javascript %}
i=(RecordCount%PageSize==0)?(RecordCount/PageSize):(this.FormatNum((RecordCount/PageSize),1,0,0,0)+1);
{% endhighlight %}
2.
{% highlight javascript %}
i= Math.ceil(RecordCount / PageSize);
{% endhighlight %}
