---
layout:     post
title:      "Do not lose hope."
subtitle:   "关于Linux安装mysql等"
date:       2017-10-24 23:00:00
author:     "Lucas Lu"
header-img: "img/post-bg-11.jpg"
---

<p>照例交代一下近况，好久没有更新博客了，我都已经是一个废人了。哈哈哈哈，最近因为怕死办了个健身卡。

	言归正传，最近买了一年的VPS当然是linux，尝试着安装了一下mysql，遇到了一些问题，在这里暂且记录一下。</p>

<p>首先按照apt命令语句从网络安装mysql：

<b> apt-get install mysql-server </b>。

如果无法检索到包可以先尝试通过<b>apt-get update</b>更新源，再次安装仍旧报错可能是源的问题可以尝试更换成淘宝的源。</p>

<p>如果网络安装失败可以通过oracle官网进行下载安装，选择对应的版本通过ssh导入linux解压安装，如果提示libaio不存在，则先安装libaio，最后安装成功。</p>

<p>安装流程如下：

进入安装mysql软件目录：执行命令 cd /usr/local/mysql

修改当前目录拥有者为mysql用户：执行命令 chown -R mysql:mysql ./

安装数据库：执行命令 ./scripts/mysql_install_db --user=mysql

修改当前目录拥有者为root用户：执行命令 chown -R root:root ./

修改当前data目录拥有者为mysql用户：执行命令 chown -R mysql:mysql data

到此数据库安装完毕
</p>

<p>
安装完可以输入mysql -uroot -p，然后enter password来进入mysql。

进去后可以使用mysql的各种语法。
</p>