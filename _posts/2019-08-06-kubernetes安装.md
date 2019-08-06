---
layout:     post   				    # 使用的布局（不需要改）
title:    	 Kubernetes 安装		# 标题 
subtitle:     #副标题
date:       2019-08-06				# 时间
author:     walikrence 						# 作者
header-img: img/post-bg-2015.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - kubernetes
    - 国内环境
---

# Kubernetes 安装

## 安装docker
```sh
sudo apt install curl -y
curl -fsSL get.docker.com -o get-docker.sh
sudo sh get-docker.sh --mirror Aliyun
sudo systemctl enable docker
sudo systemctl start docker
sudo groupadd docker
sudo usermod -aG docker $USER
```
```sh
sudo usermod -aG docker <你的用户名>
```

docker使用镜像

```sh
vim /etc/docker/daemon.json
```
修改成
```json
{
  "registry-mirrors": [
    "https://registry.docker-cn.com"
  ]
}
```
```sh
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 关闭交换
```sh
sudo swapoff -a
```
永久关闭交换
```sh
vim /etc/fstab
```

```vim
UUID=42cf9d46-ae0d-488d-9e3a-b6f78708bea1 /               ext4    errors=remount-ro 0       1
#/swapfile                                 none            swap    sw              0       0
```
修改后重启

## 安装kubelet kubeadm kubectl
```sh
apt-get update && apt-get install -y apt-transport-https curl
cat <<EOF > /etc/apt/sources.list.d/kubernetes.list
deb http://mirrors.ustc.edu.cn/kubernetes/apt kubernetes-xenial main
EOF
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 6A030B21BA07F4FB

apt-get update
apt-get install -y kubelet kubeadm kubectl
apt-mark hold kubelet kubeadm kubectl
```

```sh
sudo vim /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

在末尾添加
```
Environment="KUBELET_CGROUP_ARGS=--cgroup-driver=cgroupfs"
```
```sh
systemctl daemon-reload
systemctl restart kubelet
```

## 关闭防火墙

```sh
vim /etc/sysctl.conf
```
在末尾添加
```sh
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
```

## 下载需要的镜像并修改标签
```sh
docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.15.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-apiserver:v1.15.1 k8s.gcr.io/kube-apiserver:v1.15.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.15.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-controller-manager:v1.15.1 k8s.gcr.io/kube-controller-manager:v1.15.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.15.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-scheduler:v1.15.1 k8s.gcr.io/kube-scheduler:v1.15.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.15.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kube-proxy:v1.15.1 k8s.gcr.io/kube-proxy:v1.15.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause:3.1 k8s.gcr.io/pause:3.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.3.10

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/etcd:3.3.10 k8s.gcr.io/etcd:3.3.10

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.3.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/coredns:1.3.1 k8s.gcr.io/coredns:1.3.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/pause-amd64:3.1 k8s.gcr.io/pause-amd64:3.1

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/ k8s.gcr.io/

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/kubernetes-dashboard-amd64:v1.10.0

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/kubernetes-dashboard-amd64:v1.10.0 k8s.gcr.io/kubernetes-dashboard-amd64:v1.10.0

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-amd64:v1.5.4 k8s.gcr.io/heapster-amd64:v1.5.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-grafana-amd64:v5.0.4 k8s.gcr.io/heapster-grafana-amd64:v5.0.4

docker pull registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2

docker tag registry.cn-hangzhou.aliyuncs.com/google_containers/heapster-influxdb-amd64:v1.5.2 k8s.gcr.io/heapster-influxdb-amd64:v1.5.2

```

---

以上所有步骤在所有master和slave机器上完成。


# master节点步骤

完成上面步骤后
```sh
sudo kubeadm init --kubernetes-version=v1.15.1  --pod-network-cidr=192.168.0.0/16
```
初始化master节点

拉取网络插件calico镜像
```sh
docker pull quay.io/calico/node:v3.1.3
docker pull quay.io/calico/cni:v3.1.3
docker pull quay.io/calico/typha:v0.7.4
```

配置网络插件
```sh
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/rbac-kdd.yaml
kubectl apply -f https://docs.projectcalico.org/v3.1/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
```

配置完成后可能ssh就断开了，因为calico的网段也是192.168.0.0。暂时没有什么很好的解决方案，但是通过连在同一个交换机上的电脑仍然可以ssh连接或者直接操作master节点。

## 打印加入集群的命令
```sh
kubeadm token create --print-join-command
```

```sh

```


# slave节点

完成初始化步骤后，通过master节点打印出的加入集群命令加入集群


