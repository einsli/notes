# Kubernets 学习笔记

## 第一节 kuebernets 概述

## 1.1 K8s是什么

- kubernets简称k8s;
- k8s本质上是一组服务器集群，k8s可以在集群的各个节点上运行特定的docker容器；
- K8s用于容器化应用程序部署，扩展和管理；
- k8s是Google在2014年开园的一个容器集群管理系统；
- k8s提供了容器编排，资源调度，弹性伸缩，服务管理。服务发现等一系列功能；
- k8s目标是让部署容器化应用简单高效。

## 1.2 k8s功能

- 自我修复
- 弹性伸缩: 根据服务器的并发情况，并发或者缩减容器数量
- 自动部署和回滚: yaml文件
- 服务发现和负载均衡
- 机密和配置管理

## 1.3 k8s 节点

k8s分为两类结点:

- master node：主结点
- work node：工作结点

### 1.3.1 master 节点的组件(程序)

- Api Server：集群的统一入口，各组件的协调者，以RestFul API提供接口服务，所有对象资源的增删改查和监听等操作都交给API Server处理后再提交给Etcd存储 (接收客户端操作可执行的k8s命令)；
- scheduler：根据调度算法为新创建的Pod选择一个Node节点，可以任意部署，可以部署在同一个节点上，也可以部署在不同节点上（从多个work node节点的组件中选举一个来启动服务)；
- controller manager：处理集群中常规后台任务，一个资源对应一个控制器，而controller manager就是负责管理这些控制器的（向work节点的kubelet发送指令);
- etcd: 分布式键值存储系统，用于保存集群状态数据，比如Pod，Service等对象信息（欢聚话说即 k8s数据库，用来注册节点、服务、记录账号等一系列操作）

### 1.3.2 node 节点的组件(程序)

- kubelet：kubectl是master在node节点上的agent，管理本机运行容器的声明周期，比如创建容器、Pod挂载数据卷、下载secret、获取容器和结点状态等工作，kubelet将每个pod转换成一组容器（向docker发送指令管理docker容器的）；
- kubeproxy：在node节点上实现pod网络代理，维护网络规则和四层负载均衡工作(管理docker容器的网络)；
- docker或rocket：容器引擎，运行容器。

**k8s架构图下**

![avatar](../assets/k8s-notes/k8s-structure.png)

容器启动的一个过程:

- 客户端向API Server发送请求，即kubectl → API Server
- API Server收到请求后，会向scheduler发送指令，即API Server→scheduler
- scheduler会向后端若干个结点中寻找一个节点（例如节点/node1），即schedule→（寻找node结点）节点/node
- scheduler寻找到节点后，scheduler会将结果返回API Server 即scheduler→API Server
- API Server接收到scheduler返回的寻找的结点后，会传递给controller manager 即API Server → controller manager
- controller manager 会向选定的结点（例如节点/node1）发送指令消息，即controller→kubectl
- kubectl 收到controller传递过来的信息后，会向本地主机docker发送指令,启动一个容器（即pod），即kubectl→（本地）docker

## 1.4 pod 概念(重要)

- 最小部署单元
- 一个pod可以有一个或者多个容器——容器组
- 一组容器的集合
- 一个pod中的容器共享网络命名空间
- pod是短暂的

**注:**

k8是否可以直接启动容器？

答: 不能，因为k8s里面最小的调度单元是pod。

## 1.5 controllers

控制器，控制pod，启动、停止、删除pod

- ReplicaSet：确保预期的pod副本数数量
- Deployment：无状态应用部署
- StatefulSet：有状态应用部署
- DaemonSet：确保所有node运行同一个pod
- Job：一次性任务
- Cronjob：定时任务

## 1.6 Service

将一组pod关联起来，提供一个统一的入口，及时pod地址发生改变，这个统一入口也不会变化，可以保证用户访问不受影响。

- 防止pod失联；
- 定义一组pod的访问策略。

**service 内部流程**

![avatar](../assets/k8s-notes/service-process.png)

## 1.7 Label

一组pod是一个统一的标签，service是通过标签和一组pod进行关联的。

- 标签，附加到某个资源上，用于关联对象、查询和筛选。

**label图示(Services下面的名字如nginx即为label)**

![avatar](../assets/k8s-notes/label-notes.png)

## 1.8 Namespace

用来隔离pod运行空间(默认情况下，pod是可以互相访问的)；

使用场景：

- 为不同的公司提供隔离的pod运行环境；
- 为开发环境、测试环境、生产环境分别准备不同的命名空间，进行隔离。