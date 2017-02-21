---
layout:     post
title:      "The bird caught a bug on the fly"
subtitle:   "Review knowledge about CURSOR"
date:       2017-02-21 22:0:00
author:     "Lucas Lu"
header-img: "img/post-bg-07.jpg"
---

<p>今天在工作的时候发现某个视图读取速度特别慢，仔细看了一下该视图的SQL代码，发现里面有一个Event同步表数据特别多（约2千万），另外还Join了两张表Door和Card。</p>

<p>最先考虑到在这张表里加索引，多加了几个索引之后。再次尝试，速度快了一些，但还是很慢。再想了想，既然是同步表，索性在每天同步的时候直接Join两张表。</p>

<p>接着又想到这么操作导致同步的时候可能时间太久，就想到用游标来逐条插入数据。正巧刚跟人开玩笑说肯定不记得SQL游标怎么写了。接着就产生了以下：</p>

<blockquote><xmp>
SET @StartTime = Convert(varchar(100),GETDATE()-1,23)
SET @EndTime = Convert(varchar(100),GETDATE(),23)
SET @Sql = 'SELECT cardno,eventtime,doorno from `event` 
where eventtime> ''' + @StartTime +''' and eventtime<''' + @EndTime  +''''
SET @Sql = 'DECLARE ENTRYRECORDS CURSOR FORWORD_ONLY READ_ONLY FOR 
SELECT * FROM OPENQUERY([OtherDataSource], ''' + REPLACE(@Sql, '''', '''''') + ''')'
EXEC(@Sql)
OPEN ENTRYRECORDS
FETCH ENTRYRECORDS
INTO @CardNO,@Eventtime,@DoorNo
WHILE @@FETCH_STATUS = 0
BEGIN
  INSERT Event	  (door,card,eventTime) 
  SELECT d.name,c.name,@Eventtime
  FROM door d 
  JOIN	card c on c.cardno = @CardNo
  WHERE d.doorno = @DoorNo  
  FETCH NEXT FROM ENTRYRECORDS
  INTO @CardNO,@Eventtime,@DoorNo
END
CLOSE ENTRYRECORDS
DEALLOCATE ENTRYRECORDS
</xmp></blockquote>

<p>可以在以上的代码中看到因为OPENQUERY不支持参数，所以通过EXEC SQL的方式进行插入游标，又因为这个方式(EXEC作用在不同的工作域)只能定义全局游标(一般Local游标会好一点)，FORWORD_ONLY／READ_ONLY则可以游标的效率稍高一些。</p>

<p>经过这样的改写，直接从同步表读取数据，读取的速度提升了近四倍。</p>
