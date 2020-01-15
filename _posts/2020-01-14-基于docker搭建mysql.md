---
layout:     post   				    # 使用的布局（不需要改）
title:      基于docker搭建mysql			# 标题
subtitle:     #副标题
date:       2020-01-14 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - nexus
    - maven
    - Docker
    - java

---

# 基于docker搭建mysql	

## 下载镜像
```sh 
docker pull mysql:5.7
```

## 编写docker-compose.yml
```yml
version: '3'

services:

  db:
    image: mysql:5.7
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
     - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: 123456
```

## 启动
```sh 
docker-compose up -d
```