---
layout:     post   				    # 使用的布局（不需要改）
title:      a start job is running for wait for network to be configured   		# 标题
subtitle:     #副标题
date:       2020-01-16 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - ubuntu
    - netplan
    - virtual box

---

# a start job is running for wait for network to be configured	

系统: ubuntu-18.04.3-live-server-amd64

a start job is running for wait for network to be configured
今天虚拟机开机遇到一个问题，当我拔掉网卡网线后,开机就出现一个读条

![](../../../../img/2018-01-16/1.png) <br>

## 修改 /etc/netplan/50-cloud-init.yaml

```sh 
vim /etc/netplan/50-cloud-init.yaml
```
添加 optional: true
```yml
network:
    network:
        ethernets:
            enp0s3:
                addresses:
                        - 192.168.56.20/24
                optional: true
            enp0s8:
                dhcp4: yes
        version: 2
```

## 使用
```sh 
netplan apply
```
读条消失
## 参考资料
[运维之美](https://www.hi-linux.com/posts/49513.html)