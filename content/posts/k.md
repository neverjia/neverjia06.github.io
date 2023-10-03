+++
title = '笔记'
date = 2023-10-02T21:15:18+08:00
draft = false
+++
另外，已经有很多 PaaS 系统运行在 Kubernetes 之上，如 Openshift, Deis 和 Eldarion 等。 您也可以构建自己的 PaaS 系统，或者只使用 Kubernetes 管理您的容器应用。
当然了，Kubernetes 不仅仅是一个 “编排系统”，它消除了编排的需要。Kubernetes 通过声明式的 API 和一系列独立、可组合的控制器保证了应用总是在期望的状态，而用户并不需要关心中间状态是如何转换的。这使得整个系统更容易使用，而且更强大、更可靠、更具弹性和可扩展性。
核心组件
Kubernetes 主要由以下几个核心组件组成：
etcd 保存了整个集群的状态；
apiserver 提供了资源操作的唯一入口，并提供认证、授权、访问控制、API 注册和发现等机制；
controller manager 负责维护集群的状态，比如故障检测、自动扩展、滚动更新等；
scheduler 负责资源的调度，按照预定的调度策略将 Pod 调度到相应的机器上；
kubelet 负责维护容器的生命周期，同时也负责 Volume（CVI）和网络（CNI）的管理；
Container runtime 负责镜像管理以及 Pod 和容器的真正运行（CRI）；
kube-proxy 负责为 Service 提供 cluster 内部的服务发现和负载均衡
![Alt text](image.png)



Container
Container（容器）是一种便携式、轻量级的操作系统级虚拟化技术。它使用 namespace 隔离不同的软件运行环境，并通过镜像自包含软件的运行环境，从而使得容器可以很方便的在任何地方运行。


Pod
Kubernetes 使用 Pod 来管理容器，每个 Pod 可以包含一个或多个紧密关联的容器。
Pod 是一组紧密关联的容器集合，它们共享 IPC 和 Network namespace，是 Kubernetes 调度的基本单位。Pod 内的多个容器共享网络和文件系统，可以通过进程间通信和文件共享这种简单高效的方式组合完成服务。