---
layout:     post   				    # 使用的布局（不需要改）
title:      基于docker自建docker-registry前端			# 标题
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

# registry前端	

系统: ubuntu-18.04.3-live-server-amd64

## 下载nexus镜像
```sh 
docker pull registry:2.7
docker pull konradkleine/docker-registry-frontend:v2
```
## 编写docker-compose.yml
docker-compose.yml
```sh 
version: '3'
services:
    registry:
        image: 'registry:2.7'
        restart: always
        ports:
            - 5000:5000
        volumes:
            - data:/var/lib/registry
    frontend:
        image: konradkleine/docker-registry-frontend:v2
        restart: always
        ports:
            - 80:80
        environment:
            - ENV_DOCKER_REGISTRY_HOST=172.20.10.3
            - ENV_DOCKER_REGISTRY_PORT=5000
volumes:
    data:
```

## 启动

```sh 
docker-compose up -d
```

