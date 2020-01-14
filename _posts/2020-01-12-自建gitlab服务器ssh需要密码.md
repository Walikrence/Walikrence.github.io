---
layout:     post   				    # 使用的布局（不需要改）
title:      自建gitlab服务器ssh需要密码			# 标题
subtitle:     #副标题
date:       2020-01-12 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - 虚拟机
    - gitlab
    - Docker

---

# 自建gitlab服务器ssh需要密码	

系统: ubuntu-18.04.3-live-server-amd64

今天基于docker搭建了gitlab服务器,配置了ssh密钥，但是ssh下载的时候提示需要密码
![](../../../../img/自建gitlab服务器ssh需要密码/1.png)
排查发现ssh在gitee和github上都可用，但是就是自建的gitlab不可用。

发现命令需要加端口号
```sh 
git clone ssh://git@172.20.10.8:2222/walikrence/hello-gitlab.git
```
![](../../../../img/自建gitlab服务器ssh需要密码/2.png)

但是这样每次手动改git clone 命令太麻烦了，查资料发现可以通过修改docker-compose.yml 来指定ssh端口，让gitlab直接自动生成上面的命令

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
设置ssh端口为2222
```yml
          gitlab_rails['gitlab_shell_ssh_port'] = 2222
```
修改完后就能正常使用了2020-01-12-自建gitlab服务器ssh需要密码.md