---
layout:     post   				    # 使用的布局（不需要改）
title:      基于docker搭建gitlab服务器			# 标题
subtitle:     #副标题
date:       2020-01-12 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - gitlab
    - Docker

---

# 基于docker搭建gitlab服务器

系统: ubuntu-18.04.3-live-server-amd64

今天想基于docker搭建一个gitlab服务器,以下步骤基于已有docker和docker-compose

## 找到一个docker image

[gitlab中文社区版](https://hub.docker.com/r/twang2218/gitlab-ce-zh)

## 找到一个合适的版本并下载
```sh 
docker pull twang2218/gitlab-ce-zh:11.1
```

## 写一个docker compose
```yml
version: '3'
services:
    gitlab:
      image: 'twang2218/gitlab-ce-zh:11.1'
      restart: always
      hostname: '172.20.10.8'
      environment:
        TZ: 'Asia/Shanghai'
        GITLAB_OMNIBUS_CONFIG: |
          external_url 'http://172.20.10.8'
          gitlab_rails['time_zone'] = 'Asia/Shanghai'
          gitlab_rails['gitlab_shell_ssh_port'] = 2222
      ports:
        - '80:80'
        - '8443:443'
        - '2222:22'
      volumes:
        - config:/etc/gitlab
        - data:/var/opt/gitlab
        - logs:/var/log/gitlab
volumes:
    config:
    data:
```
## 启动
```sh 
docker-compose up -d
```

## 关闭
```sh 
docker-compose down
```