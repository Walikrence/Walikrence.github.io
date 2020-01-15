---
layout:     post   				    # 使用的布局（不需要改）
title:      基于docker自建nexus私服			# 标题
subtitle:     #副标题
date:       2020-01-13 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - nexus
    - maven
    - Docker
    - java

---

# 基于docker自建nexus私服	

系统: ubuntu-18.04.3-live-server-amd64

## 下载nexus镜像
```sh 
docker pull sonatype/nexus3:3.20.1
```
## 编写docker-compose.yml
docker-compose.yml
```sh 
version: '3'
services:
    nexus:
        image : 'sonatype/nexus3:3.20.1'
        restart: always
        ports:
            - 8081:8081
        volumes:
            - data:/nexus-data
volumes:
    data:
```

## 启动

```sh 
docker-compose up -d
```

