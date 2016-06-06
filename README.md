# catchup

https://github.com/k8sp 想做的事情是基于Kubernetes搭建一个通用计算机群。
我们想做的具体的工作列在[这里](https://github.com/k8sp/issues/issues)。

## Pull Request

我们的成员分布在不同公司不同大路上，为了方便集思广益，我们的所有工作流
程都在Github上。所以了解Github尤其是Pull Request是一个很重的前提。

- https://yangsu.github.io/pull-request-tutorial/

## Vagrant

因为很多机群的配置工作都是在踩坑。如果用物理机器（bare metal），一来系
统重装很慢，二来资源有限。一个高效的方式是用Vagrant在本机创建一个虚拟
机群。

- https://github.com/k8sp/vagrant/blob/master/what_is_vagrant.md
- https://www.vagrantup.com/docs/

需要注意的是，我们的“本机”最好是一个Linux或者Mac机器。

## Docker

Kubernetes只能调度 containers，不能运行一般的二进制可执行文件，所以需
要了解Docker container。

- https://zhuanlan.zhihu.com/p/19902938?refer=cxwangyi
- https://docs.docker.com/mac/

## CoreOS

因为我们要管理和操作服务器机群，所以每台机器上得安装一个服务器操作系统。
目前世界上唯一的“服务器Linux操作系统”是CoreOS。其他诸如CentOS等“号称”
服务器操作系统的，其实都是桌面系统。只有桌面系统才需要package
management system，例如 apt 和 yum。因为桌面系统只有一个或者少数几个人
用，大家能预期需要在上面安装什么软件，而packages是这些软件或者其依赖的
组建。在一个机群上，分布式操作系统（例如Kubernetes）有可能把任何一个用
户的任何一个作业调度到一台机器上。有的作业需要Python 2.4，有的需要
Python 3，有的需要Java 8，有的需要OpenCV。要在所有机器都预先安装好这些
依赖，几乎是不可能的。一个“真的”服务器操作系统，只需要能运行containers，
每个container里面包含需要的所有依赖。CoreOS就是这样一个操作系统。

- http://www.infoworld.com/article/2692889/open-source-software/coreos-an-existential-threat-to-linux-vendors.html
- https://coreos.com/os/docs/latest/

### systemd

CoreOS里预装的极少的几个软件里，systemd是一个。systemd是替换传统Unix操
作系统中的initd的——作为操作系统启动的第一个进程，用来启动和管理其他重
要的系统服务和进程。在CoreOS里，Kubernetes依赖的几个重要基础服务都是用
systemd来启动的。

- http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

### etcd

etcd 是 Kubernetes 依赖的最重要的基础服务之一。etcd维护一个分布式
key-value pair存储。这个存储服务的特点是它“不会死”。我们机群上所有其他
分布式系统（或者子系统，包括Kubernetes自己）如果要设计成“不会死”，都需
要利用etc。

- https://github.com/k8sp/etcd
- https://github.com/coreos/etcd/tree/master/Documentation/v2

### flannel

Kubernetes的一个重要特性是，每个计算单元（称为pod，实际上是一个
pod-typed docker container）都有自己的虚拟IP地址。这个IP地址和物理机器
的IP地址无关。从而使得分布式软件系统开发和部署非常简单。这组虚拟IP地址
的分配和管理，是通过另一个基础服务flanneld实现的。注意：flanneld依赖
etcd。

- https://github.com/k8sp/kubernetes/tree/master/networking
- https://xelatex.github.io/2015/10/10/Flannel-for-Docker-Overlay-Network/
- https://github.com/coreos/flannel

## Kubernetes

- https://github.com/k8sp/kubernetes
- https://github.com/k8sp/kubernetes/tree/master/networking

