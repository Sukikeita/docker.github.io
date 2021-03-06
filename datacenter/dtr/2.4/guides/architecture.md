---
title: DTR architecture
description: Learn about the architecture of Docker Trusted Registry.
了解DTR的架构
keywords: registry, dtr, architecture
注册服务器 dtr 架构
---

Docker Trusted Registry (DTR) is a containerized application that runs on a
Docker Universal Control Plane cluster.

Docker可信注册表（DTR）是在Docker通用控制平面群集上运行的集装箱式应用程序。

![](images/architecture-1.svg)

Once you have DTR deployed, you use your Docker CLI client to login, push, and
pull images.

一旦安装了DTR，你使用你的Docker　CLI客户端登陆、推送和拉取镜像。

## Under the hood 引擎盖下

For high-availability you can deploy multiple DTR replicas, one on each UCP
worker node.

为获得高可用性，您可以部署多个DTR副本，每个UCP工作节点上有一个副本。

![](images/architecture-2.svg)

All DTR replicas run the same set of services and changes to their configuration
are automatically propagated to other replicas.

所有DTR副本都运行相同的一组服务，并且对其配置的更改会自动传播到其他副本。


## DTR internal components DTR内部组成

When you install DTR on a node, the following containers are started:
当你吧把DTR安装在一个节点山个，以下容器将启动

| Name                                 | Description                                                                                                                        |
|:-------------------------------------|:-----------------------------------------------------------------------------------------------------------------------------------|
| dtr-api-&lt;replica_id&gt;           | Executes the DTR business logic. It serves the DTR web application, and API                                                        
|
| dtr-garant-&lt;replica_id&gt;        | Manages DTR authentication                                                                                                         |
| dtr-jobrunner-&lt;replica_id&gt;     | Runs cleanup jobs in the background                                                                                                |
| dtr-nginx-&lt;replica_id&gt;         | Receives http and https requests and proxies them to other DTR components. By default it listens to ports 80 and 443 of the host   |
| dtr-notary-server-&lt;replica_id&gt; | Receives, validates, and serves content trust metadata, and is consulted when pushing or pulling to DTR with content trust enabled |
| dtr-notary-signer-&lt;replica_id&gt; | Performs server-side timestamp and snapshot signing for content trust metadata                                                     |
| dtr-registry-&lt;replica_id&gt;      | Implements the functionality for pulling and pushing Docker images. It also handles how images are stored                          |
| dtr-rethinkdb-&lt;replica_id&gt;     | A database for persisting repository metadata                                                                                      |
| dtr-scanningstore-&lt;replica_id&gt; | Stores security scanning data                                                                                                      |

All these components are for internal use of DTR. Don't use them in your applications.

## Networks used by DTR

To allow containers to communicate, when installing DTR the following networks
are created:

| Name   | Type    | Description                                                                            |
|:-------|:--------|:---------------------------------------------------------------------------------------|
| dtr-ol | overlay | Allows DTR components running on different nodes to communicate, to replicate DTR data |


## Volumes used by DTR

DTR uses these named volumes for persisting data:

| Volume name                         | Description                                                                      |
|:------------------------------------|:---------------------------------------------------------------------------------|
| dtr-ca-&lt;replica_id&gt;           | Root key material for the DTR root CA that issues certificates                   |
| dtr-notary-&lt;replica_id&gt;       | Certificate and keys for the Notary components                                   |
| dtr-postgres-&lt;replica_id&gt;     | Vulnerability scans data                                                         |
| dtr-registry-&lt;replica_id&gt;     | Docker images data, if DTR is configured to store images on the local filesystem |
| dtr-rethink-&lt;replica_id&gt;      | Repository metadata                                                              |
| dtr-nfs-registry-&lt;replica_id&gt; | Docker images data, if DTR is configured to store images on NFS                  |

You can customize the volume driver used for these volumes, by creating the
volumes before installing DTR. During the installation, DTR checks which volumes
don't exist in the node, and creates them using the default volume driver.

By default, the data for these volumes can be found at
`/var/lib/docker/volumes/<volume-name>/_data`.

## Image storage

By default, Docker Trusted Registry stores images on the filesystem of the node
where it is running, but you should configure it to use a centralized storage
backend.

![](images/architecture-3.svg)

DTR supports these storage backends:

* NFS
* Amazon S3
* Cleversafe
* Google Cloud Storage
* OpenStack Swift
* Microsoft Azure

## How to interact with DTR

DTR has a web UI where you can manage settings and user permissions.

![](images/architecture-4.svg)

You can push and pull images using the standard Docker CLI client or other tools
that can interact with a Docker registry.

## Where to go next

* [System requirements](admin/install/system-requirements.md)
* [Install DTR](admin/install/index.md)
