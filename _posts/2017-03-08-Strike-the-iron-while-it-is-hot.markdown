---
layout:     post
title:      "Strike the iron while it is hot."
subtitle:   "千里之行始于足下"
date:       2017-03-28 23:00:00
author:     "Lucas Lu"
header-img: "img/post-bg-10.jpg"
---

<p>这两天安装了Microsoft WebMatrix3，搜索了Orchard CMS开始安装。一如既往的开始报错，嗯，让我们看看是什么问题呀？</p>

<p>哦，IIS8安装失败，您已安装高于此版本的IIS。哦，对，装VS2015的时候顺便装了IIS10，行，不兼容是吧，卸了。重新安装，好的，成功。</p>

<p>那么让朋友我看看这玩意到底是个啥，很了不起吗？号称能让人从.NET入门到放弃？呃...这框架还行啊，让我们看看代码。既然是MVC嘛，又不是没学过，咱研究下....</p>

<a href="#">
    <img src="{{ site.baseurl }}/img/post-orchard-struct.png" alt="Post Orchard Struct">
</a>

<p>WTF?!!这玩意什么鬼？这都什么呀，页面呢？View呢？Controller呢？？哦，在Module里...</p>

<p>....总之过了几天，努力的啃了一段时间官方文档，另外感谢Orchard中文网，我对Orchard总算有了一些简单的认识。比较喜欢的是Orchard的热插拔模式，它提供了</p>


