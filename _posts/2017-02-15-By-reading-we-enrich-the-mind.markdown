---
layout:     post
title:      "By reading we enrich the mind, by conversation we polish it"
subtitle:   "learn from my workmate what about Linq"
date:       2017-02-15 22:0:00
author:     "Lucas Lu"
header-img: "img/post-bg-06.jpg"
---

<p>2014年第一次接触Linq时，对MVC一知半懂，对Linq也没什么理解。到了2016重拾MVC，Entity Framework，是在用ASP.NET开发一年之后，用惯了数据库语句处理各种数据，对Entity Framework中的一些BUG各种吐槽，
不过没关系，写啊写啊就习惯了。</p>

<p>当我们习惯了使用LINQ的时候，就会发现LINQ使很多数据操作变得极其简单，典型的一个例子就是foreach操作。比如说我们要输出某个对象集合的某一个属性的列表值，最通常的情况下想到的是:</p>

<blockquote>
	foreach(var Obj in ObjList){
	    result += Obj.value;
    }
</blockquote>

<p>但是我们使用LINQ时可以像写<strong>select value from ObjList</strong>这样一句话写出，也许有人说像LINQ<strong>var result = ObjList.Select(o => o.value)</strong>其实差别并不大。</p>

<p>可是如果我们取出来的数据是多列的，加上各种条件(不重复/value>3)的呢？</p>

<blockquote>
	var result = ObjList.Where(o => o.value > 3).Select(o => new {value1 = o.value1, value2 = o.value2, value3 = o.value3}).distinct();
</blockquote>

<p>像上面这样我们甚至偷懒少写了一个对象。虽然LINQ TO Enitty实际上没有提升太大的效率，只是让代码的可读性以及简洁性变高了，但是对于那些刚入坑的新手来说LINQ实在是福音。
对于新手来说一段优美简洁的代码如同一座明亮的灯塔，可以激励一生。</p>
