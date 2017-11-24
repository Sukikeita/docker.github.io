---
title: DTR backups and recovery
description: Learn how to back up your Docker Trusted Registry cluster, and to recover your cluster from an existing backup.了解如何备份您的Docker Trusted Registry集群，并从现有备份中恢复您的集群。

keywords: registry, high-availability, backup, recovery 注册 高性能 备份 恢复
---

{% assign image_backup_file = "backup-images.tar" %}
{% assign metadata_backup_file = "backup-metadata.tar" %}

DTR requires that a majority (n/2 + 1) of its replicas are healthy at all times
for it to work. So if a majority of replicas is unhealthy or lost, the only
way to restore DTR to a working state, is by recovering from a backup. This
is why it's important to ensure replicas are healthy and perform frequent
backups.

DTR要求其复制品的大多数（n / 2 + 1）在任何时候都健康地工作。 因此，如果大多数副本不健康或丢失，将DTR恢复到工作状态的唯一方法是从备份恢复。 这就是为什么重要的是要确保副本是健康的，并执行频繁的备份。

## Data managed by DTR DTR管理的数据

Docker Trusted Registry maintains data about: DTR维护以下数据：

| Data                               | Description                                                                                                                                       |
|:-----------------------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------|
| Configurations                     | The DTR cluster configurations DTR群集配置|
| Repository metadata                | The metadata about the repositories and images deployed  存储库和镜像的元数据|
| Access control to repos and images | Permissions for teams and repositories团队和存储库的权限|
| Notary data                        | Notary tags and signatures公证标签和签名|
| Scan results                       | Security scanning results for images镜像的安全扫描结果|
| Certificates and keys              | The certificates, public keys, and private keys that are used for mutual TLS communication用于相互TLS通信的证书，公钥和私钥|
| Images content                     | The images you push to DTR. This can be stored on the filesystem of the node running DTR, or other storage system, depending on the configuration您推送给DTR的镜像。 这可以存储在运行DTR或其他存储系统的节点的文件系统上，具体取决于配置|

This data is persisted on the host running DTR, using named volumes.
这些数据在使用命名卷运行DTR的主机上保存。 了解有关DTR命名卷的更多信息。
[Learn more about DTR named volumes](../architecture.md).

To perform a backup of a DTR node, run the `docker/dtr backup` command. This
command backups up the following data:
要执行DTR节点的备份，请运行`docker/dtr backup`命令。 该命令备份以下数据：


| Data                               | Backed up | Description                                                    |
|:-----------------------------------|:----------|:---------------------------------------------------------------|
| Configurations                     | yes       | DTR settings DTR设置                                           |
| Repository metadata                | yes       | Metadata like image architecture and size 保存镜像结果和大小等元数据 |
| Access control to repos and images | yes       | Data about who has access to which images  保存镜像及其访问权限的数据|
| Notary data                        | yes       | Signatures and digests for images that are signed 对已签名的镜像进行签名和摘要数据|
| Scan results                       | yes       | Information about vulnerabilities in your images 有关您的镜像中的漏洞的信息|
| Certificates and keys              | yes       | TLS certificates and keys used by DTR TLS证书和DTR使用的密钥|
| Image content                      | no        | Needs to be backed up separately, depends on DTR configuration 需要分别备份的镜像内容，取决于DTR配置|
| Users, orgs, teams                 | no        | Create a UCP backup to backup this data 创建UCP备份以备份user、org和team数据|
| Vulnerability database             | no        | Can be re-downloaded after a restore 在恢复后重新下载|


## Backup DTR data 备份DTR数

To create a backup of DTR you need to:

1. Backup image content
2. Backup DTR metadata

You should always create backups from the same DTR replica, to ensure a smoother
restore.

### Backup image content

Since you can configure the storage backend that DTR uses to store images,
the way you backup images depends on the storage backend you're using.

If you've configured DTR to store images on the local filesystem or NFS mount,
you can backup the images by using ssh to log into a node where DTR is running,
and creating a tar archive of the [dtr-registry volume](../architecture.md):

```none
{% raw %}
sudo tar -cf {{ image_backup_file }} \
$(dirname $(docker volume inspect --format '{{.Mountpoint}}' dtr-registry-<replica-id>))
{% endraw %}
```

If you're using a different storage backend, follow the best practices
recommended for that system.


### Backup DTR metadata

To create a DTR backup, load your UCP client bundle, and run the following
command, replacing the placeholders for the real values:

```none
read -sp 'ucp password: ' UCP_PASSWORD; \
docker run --log-driver none -i --rm \
  --env UCP_PASSWORD=$UCP_PASSWORD \
  {{ page.dtr_org }}/{{ page.dtr_repo }}:{{ page.dtr_version }} backup \
  --ucp-url <ucp-url> \
  --ucp-insecure-tls \
  --ucp-username <ucp-username> \
  --existing-replica-id <replica-id> > backup-metadata.tar
```

Where:

* `<ucp-url>` is the url you use to access UCP
* `<ucp-username>` is the username of a UCP administrator
* `<replica-id>` is the id of the DTR replica to backup


This prompts you for the UCP password, backups up the DTR metadata and saves the
result into a tar archive. You can learn more about the supported flags in
the [reference documentation](../../reference/cli/backup.md).

The backup command doesn't stop DTR, so that you can take frequent backups
without affecting your users. Also, the backup contains sensitive information
like private keys, so you can encrypt the backup by running:

```none
gpg --symmetric {{ backup-metadata.tar }}
```

This prompts you for a password to encrypt the backup, copies the backup file
and encrypts it.

### Test your backups

To validate that the backup was correctly performed, you can print the contents
of the tar file created. The backup of the images should look like:

```none
tar -tf {{ image_backup_file }}

dtr-backup-v{{ page.dtr_version }}/
dtr-backup-v{{ page.dtr_version }}/rethink/
dtr-backup-v{{ page.dtr_version }}/rethink/layers/
```

And the backup of the DTR metadata should look like:

```none
tar -tf {{ backup-metadata.tar }}

# The archive should look like this
dtr-backup-v{{ page.dtr_version }}/
dtr-backup-v{{ page.dtr_version }}/rethink/
dtr-backup-v{{ page.dtr_version }}/rethink/properties/
dtr-backup-v{{ page.dtr_version }}/rethink/properties/0
```

If you've encrypted the metadata backup, you can use:

```none
gpg -d /tmp/backup.tar.gpg | tar -t
```

You can also create a backup of a UCP cluster and restore it into a new
cluster. Then restore DTR on that new cluster to confirm that everything is
working as expected.

## Restore DTR data

If your DTR has a majority of unhealthy replicas, the one way to restore it to
a working state is by restoring from an existing backup.

To restore DTR, you need to:

1. Stop any DTR containers that might be running
2. Restore the images from a backup
3. Restore DTR metadata from a backup
4. Re-fetch the vulnerability database

You need to restore DTR on the same UCP cluster where you've created the
backup. If you restore on a different UCP cluster, all DTR resources will be
owned by users that don't exist, so you'll not be able to manage the resources,
even though they're stored in the DTR data store.

When restoring, you need to use the same version of the `docker/dtr` image
that you've used when creating the update. Other versions are not guaranteed
to work.

### Stop DTR containers

Start by removing any DTR container that is still running:

```none
docker run -it --rm \
  {{ page.dtr_org }}/{{ page.dtr_repo }}:{{ page.dtr_version }} destroy \
  --ucp-insecure-tls
```

### Restore images

If you had DTR configured to store images on the local filesystem, you can
extract your backup:

```none
sudo tar -xzf {{ image_backup_file }} -C /var/lib/docker/volumes
```

If you're using a different storage backend, follow the best practices
recommended for that system. When restoring the DTR metadata, DTR will be
deployed with the same configurations it had when creating the backup.


### Restore DTR metadata

You can restore the DTR metadata with the `docker/dtr restore` command. This
performs a fresh installation of DTR, and reconfigures it with
the configuration created during a backup.

Load your UCP client bundle, and run the following command, replacing the
placeholders for the real values:

```none
read -sp 'ucp password: ' UCP_PASSWORD; \
docker run -i --rm \
  --env UCP_PASSWORD=$UCP_PASSWORD \
  {{ page.dtr_org }}/{{ page.dtr_repo }}:{{ page.dtr_version }} restore \
  --ucp-url <ucp-url> \
  --ucp-insecure-tls \
  --ucp-username <ucp-username> \
  --ucp-node <hostname> \
  --replica-id <replica-id> \
  --dtr-external-url <dtr-external-url> < {{ metadata_backup_file }}
```

Where:

* `<ucp-url>` is the url you use to access UCP
* `<ucp-username>` is the username of a UCP administrator
* `<hostname>` is the hostname of the node where you've restored the images
* `<replica-id>` the id of the replica you backed up
* `<dtr-external-url>`the url that clients use to access DTR

### Re-fetch the vulnerability database

If you're scanning images, you now need to download the vulnerability database.

After you successfully restore DTR, you can join new replicas the same way you
would after a fresh installation. [Learn more](configure/set-up-vulnerability-scans.md).

## Where to go next

* [Set up high availability](configure/set-up-high-availability.md)
* [DTR architecture](../architecture.md)
