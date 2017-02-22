---
layout:     post
title:      "Today I answered Stack Overflow"
subtitle:   "Then I got a disagree"
date:       2017-02-22 22:0:00
author:     "Lucas Lu"
header-img: "img/post-bg-08.jpg"
---

<p>本着每日一答的好习惯，今天在Stack Overflow上答了一个有关数据Group By的题。这题目大致是这样的：</p>

<p>input:</p>

<pre>
Id   LocationId  UserName  Startdate
1       10        xz        2017-02-21 09:05:20
2       10        xz        2017-02-21 09:15:20
3       10        xz        2017-02-21 09:25:20
4       10        xz        2017-02-21 09:35:20
5       11        xy        2017-02-21 09:45:20
6       11        xy        2017-02-21 09:55:20
7       11        xy        2017-02-21 10:05:20
8       11        xy        2017-02-21 10:15:20
9       10        xz        2017-02-21 10:15:20
10      10        xz        2017-02-21 10:25:20
</pre>

<p>output:</p>

<pre>
Id      locationId    starttime             endtime   
1            10     2017-02-21 09:05    2017-02-21 09:35:20            
2            11     2017-02-21 09:05    2017-02-21 09:35:20  
3            10     2017-02-21 10:15    2017-02-21 10:25     
</pre>

<p>虽然感觉答不对题，姑且提供一个SQL可行版本的答案好了，一开始我的思路是这样的：先分割LocationID来进行GroupBy，但看起来还需要另一个条件：连续的。</p>

<p>我首先考虑的是，那我判断它下一ID的LocationID是不是相同，再找一个标志表示它是刚开始还是在运行过程中，于是有了以下代码：</p>

<pre>
SELECT *,
CASE WHEN LAG(LocationId,1) OVER(ORDER BY ID) != LocationId 
     OR LAG(LocationId,1) OVER(ORDER BY ID) IS NULL
     THEN ID
     ELSE 0 END FLAG
FROM [dbo].[DATATABLE])
</pre>

<p>但是这样也没办法分割啊，怎么能够把同一过程的Flag置为同一个，改造成111111222333这样的形式呢，思考了一下，马上想到了将上面那个数据表(设为Test)进行Join处理：</p>
<pre>
SELECT [LocationId],[UserName],[startdate],MAX(MI.FLAG) FLAG FROM TEST JOIN
(SELECT FLAG FROM TEST
WHERE FLAG != 0) MI ON TEST.ID >= MI.FLAG
GROUP BY [ID],[LocationId],[UserName],[startdate],TEST.FLAG)
</pre>

<p>最终很容易就搞定了结果：<strong>SELECT LocationID,MIN([startdate]),MAX([startdate]) FROM GroupData GROUP BY FLAG,LocationID</strong>
虽然我的答案很悲伤的被点了个沉贴，不过思考问题还是让人愉悦。</p>
