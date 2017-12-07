---
layout:     post
title:      "Docker vs VM"
subtitle:   ""
date:       2017-12-07
author:     "Kanon"
header-img: "https://images.pexels.com/photos/88636/pexels-photo-88636.jpeg?w=940&h=650&auto=compress&cs=tinysrgb"
tags:
    - Docker
---

先放上Docker官网上的对比原文

### Containers vs. virtual machines
Consider this diagram comparing virtual machines to containers:

#### Virtual Machine diagram
![VM](https://www.docker.com/sites/default/files/VM%402x.png)
Virtual machines run guest operating systems—note the OS layer in each box. This is resource intensive, and the resulting disk image and application state is an entanglement of OS settings, system-installed dependencies, OS security patches, and other easy-to-lose, hard-to-replicate ephemera.

#### Container diagram
![Docker](https://www.docker.com/sites/default/files/Container%402x.png)
Containers can share a single kernel, and the only information that needs to be in a container image is the executable and its package dependencies, which never need to be installed on the host system. These processes run like native processes, and you can manage them individually by running commands like `docker ps`—just like you would run ps on Linux to see active processes. Finally, because they contain all their dependencies, there is no configuration entanglement; a containerized app “runs anywhere.”
<br><br>

## 名词解释
- **Host OS**：主操作系统

- **Hypervisor**：虚拟机管理程序（Hypervisor）可以跑在主操作系统（Host OS）上，也可以直接跑在裸机硬件上，常见的VirtualBox和VMWare都是跑在主操作系统上。

- **Guest OS**：类比一个虚拟机里面可以安装多个操作系统，如Win10，Ubuntu，Guest OS指的就是在虚拟硬件资源上搭建起来的操作系统

- **Docker**：这里的Docker指代Docker守护进程(Docker Daemon)。Docker守护进程取代了Hypervisor，它是运行在操作系统之上的后台进程，负责管理Docker容器。

- **Docker镜像**：应用的源代码与所有依赖都打包在Docker镜像中，Docker容器是基于Docker镜像来创建的

## 我的理解
- 容器与虚拟机间的最大区别在于，各个容器直接共享主机系统的内核，而虚拟机则是抽象出底层的硬件资源，然后在上面跑一个个的Guest OS。

- 容器和虚拟机仅仅相似于它们都提供了隔离环境。容器能做的事少得多并且使用起来相当廉价。而虚拟机提供整个虚拟化硬件层，可以做更多的事情但是使用成本显著。

## 优缺点
- Docker优点：1.启动速度快 2.资源利用率高 3.性能开销小

- 虚拟机缺点：假设你需要运行3个相互隔离的应用，则需要使用Hypervisor启动3个Guest OS，也就是3个虚拟机。这些虚拟机都非常大，会消耗很多CPU和内存。

## 应用场景
虚拟机更擅长于彻底隔离整个运行环境。例如，云服务提供商通常采用虚拟机技术隔离不同的用户。而Docker通常用于隔离不同的应用，例如前端，后端以及数据库。
<br><b><br><br>
