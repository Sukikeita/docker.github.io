---
title: "Get Started, Part 1: Orientation and setup"
keywords: get started, setup, orientation, quickstart, intro, concepts, containers
description: Get oriented on some basics of Docker before diving into the walkthrough.
redirect_from:
- /getstarted/
- /get-started/part1/
- /engine/getstarted/
- /learn/
- /engine/getstarted/step_one/
- /engine/getstarted/step_two/
- /engine/getstarted/step_three/
- /engine/getstarted/step_four/
- /engine/getstarted/step_five/
- /engine/getstarted/step_six/
- /engine/getstarted/last_page/
- /engine/getstarted-voting-app/
- /engine/getstarted-voting-app/node-setup/
- /engine/getstarted-voting-app/create-swarm/
- /engine/getstarted-voting-app/deploy-app/
- /engine/getstarted-voting-app/test-drive/
- /engine/getstarted-voting-app/customize-app/
- /engine/getstarted-voting-app/cleanup/
- /engine/userguide/intro/
- /mac/started/
- /windows/started/
- /linux/started/
- /getting-started/
- /mac/step_one/
- /windows/step_one/
- /linux/step_one/
- /engine/tutorials/dockerizing/
- /mac/step_two/
- /windows/step_two/
- /linux/step_two/
- /mac/step_three/
- /windows/step_three/
- /linux/step_three/
- /engine/tutorials/usingdocker/
- /mac/step_four/
- /windows/step_four/
- /linux/step_four/
- /engine/tutorials/dockerimages/
- /userguide/dockerimages/
- /engine/userguide/dockerimages/
- /mac/last_page/
- /windows/last_page/
- /linux/last_page/
- /mac/step_six/
- /windows/step_six/
- /linux/step_six/
- /engine/tutorials/dockerrepos/
- /userguide/dockerrepos/
- /engine/userguide/containers/dockerimages/
---

{% include_relative nav.html selected="1" %}

Welcome! We are excited you want to learn how to use Docker.

In this six-part tutorial, you will:

1. Get set up and oriented, on this page.
2. [Build and run your first app](part2.md) 构建并运行第一个app
3. [Turn your app into a scaling service](part3.md) 把应用编程扩展服务
4. [Span your service across multiple machines](part4.md) 把服务跨越多个服务器
5. [Add a visitor counter that persists data](part5.md) 添加一个持久数据的访客计数器
6. [Deploy your swarm to production](part6.md) 热部署到生产环境

The application itself is very simple so that you are not too distracted by
what the code is doing. After all, the value of Docker is in how it can build,
ship, and run applications; it's totally agnostic as to what your application
actually does.

应用程序本身非常简单，所以你不会被代码干扰太多。 毕竟，Docker的价值在于它如何构建，发布和运行应用程序; 对于你的应用程序实际上做什么是完全不可知的。


## Prerequisites 先决条件

While we'll define concepts along the way, it is good for you to understand
[what Docker is](https://www.docker.com/what-docker) and [why you would use
Docker](https://www.docker.com/use-cases) before we begin.

虽然我们将一直定义概念，但对您来说理解[什么Docker是](https://www.docker.com/what-docker)和[为什么你会使用
Docker](https://www.docker.com/use-cases)是有益的.。

We also need to assume you are familiar with a few concepts before we continue:

在继续之前，我们还需要假设您已经熟悉了一些概念：


- IP Addresses and Ports
- Virtual Machines
- Editing configuration files
- Basic familiarity with the ideas of code dependencies and building
- Machine resource usage terms, like CPU percentages, RAM use in bytes, etc.

- IP地址和端口
- 虚拟机
- 编辑配置文件
- 基本熟悉代码依赖和构建的思想
- 机器资源使用条款，如CPU百分比，RAM使用字节数等。

## A brief explanation of containers 容器的简单介绍

An **image** is a lightweight, stand-alone, executable package that includes
everything needed to run a piece of software, including the code, a runtime,
libraries, environment variables, and config files.

**镜像**是一个轻量级独立的可执行程序包，包含运行一段软件（包括代码，运行时，库，环境变量和配置文件）所需的所有内容。


A **container** is a runtime instance of an image&#8212;what the image becomes
in memory when actually executed. It runs completely isolated from the host
environment by default, only accessing host files and ports if configured to do
so.

**容器**是一个镜像的运行时实例 - 镜像在实际执行时变成了内存。 默认情况下，它与主机完全隔离
环境，只访问主机文件和端口，如果配置了这么做。

Containers run apps natively on the host machine's kernel. They have better
performance characteristics than virtual machines that only get virtual access
to host resources through a hypervisor. Containers can get native access, each
one running in a discrete process, taking no more memory than any other
executable.

容器在主机的内核上本地运行应用程序。 它们比虚拟机具有更好的性能特征，虚拟机只能通过管理程序（hypervisor）虚拟访问主机资源的。 容器可以获得本地访问权限，每个容器都以独立的进程运行，不会占用比其他可执行文件更多的内存。


## Containers vs. virtual machines 容器 vs 虚拟机

Consider this diagram comparing virtual machines to containers:

考虑将虚拟机与容器进行比较的图表：


### Virtual Machine diagram 虚拟机

![Virtual machine stack example](https://www.docker.com/sites/default/files/VM%402x.png)

Virtual machines run guest operating systems&#8212;note the OS layer in each
box. This is resource intensive, and the resulting disk image and application
state is an entanglement of OS settings, system-installed dependencies, OS
security patches, and other easy-to-lose, hard-to-replicate ephemera.

虚拟机以访客身份操作系统 - 注意每个框中的操作系统层。 这是资源密集型的，产生的磁盘映像和应用程序状态是操作系统设置、系统安装的依赖关系、操作系统安全补丁以及其他易于丢失，难以复制的。


### Container diagram 容器

![Container stack example](https://www.docker.com/sites/default/files/Container%402x.png)

Containers can share a single kernel, and the only information that needs to be
in a container image is the executable and its package dependencies, which never
need to be installed on the host system. These processes run like native
processes, and you can manage them individually by running commands like `docker
ps`&#8212;just like you would run `ps` on Linux to see active processes.
Finally, because they contain all their dependencies, there is no configuration
entanglement; a containerized app "runs anywhere."

容器可以共享一个内核，并且唯一需要在容器镜像中的信息是可执行文件及其包依赖关系，它们永远不需要安装在主机系统上。 这些进程就像本地进程一样运行，你可以通过像docker ps这样的命令来单独管理它们，就像在Linux上运行`ps`来查看活动进程一样。 最后，因为它们包含了所有的依赖关系，所以没有配置纠缠。 一个容器的应用程序“随处运行”。


## Setup 安装

Before we get started, make sure your system has the latest version of Docker
installed.

在我们开始之前，请确保您的系统安装了最新版本的Docker。

[Install Docker](/engine/installation/index.md){: class="button outline-btn"}
<div style="clear:left"></div>
> **Note**: version 1.13 or higher is required 要求安装1.13或更高的版本。

You should be able to run `docker run hello-world` and see a response like this: 安装后，您应该可以运行 `docker run hello-world`，并且会返回以下内容
> **Note**: You may need to add your user to the `docker` group in order to call this command without sudo. [Read more](https://docs.docker.com/engine/installation/linux/linux-postinstall/) 为了不需要sudo，你可能需要添加你的user到`docker` 组。

> **Note**: If there are networking issues in your setup, `docker run hello-world` may fail to execute successfully. In case you are behind a proxy server and you suspect that it blocks the connection, check the [next part](https://docs.docker.com/get-started/part2/) of the tutorial. 如果你的安装中出现网络问题，`docker run hello-world`可能执行不成功。如果你使用代理，并且认为这是堵塞网络的原因，请查看链接中的说明

```shell
$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

此消息显示您的安装似乎正常工作。


To generate this message, Docker took the following steps:
为了生成这个消息，Docker采取了以下步骤：

...(snipped)...
```

Now would also be a good time to make sure you are using version 1.13 or higher. Run `docker --version` to check it out.

现在也是确保使用1.13或更高版本的好时机。 运行`docker --version`来检查它。

```shell
$ docker --version
Docker version 17.05.0-ce-rc1, build 2878a85
```

If you see messages like the ones above, you are ready to begin your journey.

如果你看到上面的消息，请准备开始你的旅程。

## Conclusion 结论

The unit of scale being an individual, portable executable has vast
implications. It means CI/CD can push updates to any part of a distributed
application, system dependencies are not an issue, and resource density is
increased. Orchestration of scaling behavior is a matter of spinning up new
executables, not new VM hosts.

独立、可移植的可执行文件具有广泛的含义。 这意味着CI/CD可以将更新推送到分布式应用程序的任何部分，系统依赖性不是问题，资源密度也会增加。 缩放行为的编排是关键在于新的可执行文件，而不是新的VM主机。

We'll be learning about all of these things, but first let's learn to walk.

我们将学习所有这些东西，但首先让我们学会走路。

[On to Part 2 >>](part2.md){: class="button outline-btn" style="margin-bottom: 30px; margin-right: 100%"}
