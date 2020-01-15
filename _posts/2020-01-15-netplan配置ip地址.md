---
layout:     post   				    # 使用的布局（不需要改）
title:      netplan配置ip地址   		# 标题
subtitle:     #副标题
date:       2020-01-15 				# 时间
author:     walikrence 						# 作者
header-img: img/bg/post-bg-2020.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - ubuntu
    - netplan

---

# netplan配置ip地址	

系统: ubuntu-18.04.3-live-server-amd64

## 修改 /etc/netplan/50-cloud-init.yaml

```sh 
vim /etc/netplan/50-cloud-init.yaml
```

```yml
network:
    ethernets:
        enp0s3:
            addresses:
                    - 192.168.56.10/8
            gateway4: 192.168.56.1
    version: 2
```

## 启动
```sh 
netplan apply
```

## 参考资料
[运维之美](https://www.hi-linux.com/posts/49513.html)