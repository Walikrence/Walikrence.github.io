---
layout:     post   				    # 使用的布局（不需要改）
title:      Mysql解决中文乱码				# 标题
subtitle:     #副标题
date:       2019-12-05 				# 时间
author:     walikrence 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - Mysql

---
# Mysql解决中文乱码
系统：ubuntu

最近自己搭建后台出现中文乱码情况，经过排查发现是mysql的charater_set_server默认为latin1

查看方法
```sql
show variables like '%char%'
```
修改方法
```sh
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

在末尾添加

```cnf
character_set_server = utf8
```

重启mysql

```sh
/etc/init.d/mysql restart
```