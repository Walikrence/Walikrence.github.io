---
layout:     post   				    # 使用的布局（不需要改）
title:    	搭建本地docker registry		# 标题 
date:       2019-08-08			# 时间
author:     walikrence 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - docker
---

# 搭建本地docker registry

## 服务器配置
下载registry 镜像

```sh
docker pull registry:2
```
运行
```sh

docker run -d -p 5000:5000 --name registry -v /mnt/docker/data/registry:/var/lib/registry registry:2
```

## 使用镜像的机器的配置

```sh
vim /etc/docker/daemon.json
```
添加
```json
{ "insecure-registries":    ["<运行registry服务器的ip>:5000"] }
```
例如
```json
{ "insecure-registries":    ["192.168.3.51:5000"] }
```
重启docker
```sh
systemctl restart  docker.service 
```

## 使用
```sh
docker tag nginx <ip>:5000/nginx
docker push <ip>:5000/nginx
```

检验

```sh
curl http://192.168.3.51:5000/v2/_catalog
```

打包镜像，可同时打包多个
```sh
docker save -o images.tar postgres:9.6 mongo:3.4
```
解压镜像
```sh
docker load -i images.tar
```

# k8s部署镜像方案

- 在本地部署一个registry
- 应用代码打包成tar
- 解压上传到本地registry
- 所有k8s节点配置docker的/etc/docker/daemon.json 文件使用本地registry。
- yml中的image使用<ip>:5000/<镜像名>
- 部署成功 



