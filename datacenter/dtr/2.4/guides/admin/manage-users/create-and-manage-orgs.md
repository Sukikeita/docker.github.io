---
title: Create and manage organizations 创建和管理组织
description: Learn how to set up organizations to enforce security in Docker Trusted
  Registry.
  了解如何新建组织以加强DTR安全
keywords: registry, security, permissions, organizations
注册，安全，权限，组织
---

When a user creates a repository, only that user has permissions to make changes
to the repository.

当用户创建存储库时，只有该用户有权进行更改到存储库。

For team workflows, where multiple users have permissions to manage a set of
common repositories, create an organization. By default, DTR has one
organization called 'docker-datacenter', that is shared between DTR and UCP.

对于多个用户有权管理一组公共存储库的团队工作流程，请创建一个组织。 默认情况下，DTR有一个名为“docker-datacenter”的组织，在DTR和UCP之间共享。

To create a new organization, navigate to the **DTR web UI**, and go to the
**Organizations** page.

创建组织，请查到**DTR web UI**章节，并且浏览**Organizations**页面

![](../../images/create-and-manage-orgs-1.png){: .with-border}

Click the **New organization** button, and choose a meaningful name for the
organization.
点击**New organization**按钮，并命名；

![](../../images/create-and-manage-orgs-2.png){: .with-border}

Repositories owned by this organization will contain the organization name, so
to pull an image from that repository, you'll use:

此组织所拥有的存储库将包含组织名，因此，为了拉取存储库的镜像，你要使用：

```bash
$ docker pull <dtr-domain-name>/<organization>/<repository>:<tag>
```

Click **Save** to create the organization, and then **click the organization**
to define which users are allowed to manage this
organization. These users will be able to edit the organization settings, edit
all repositories owned by the organization, and define the user permissions for
this organization.

点击**Save**以创建组织，然后点击**click the organization**定义哪些用户允许管理此组织。这些用户将可以编辑组织的设置，编辑该组织所有的存储库，并定义可访问该组织的用户。

For this, click the **Add user** button, **select the users** that you want to
grant permissions to manage the organization, and click
**Save**. Then change their permissions from 'Member' to **Admin**.

对此，点击**Add user** 按钮，选择一个你要赋予管理权限的用户，点击**Save**按钮，接着，把这些用户的权限从Member为**Admin**。

![](../../images/create-and-manage-orgs-3.png){: .with-border}

## Where to go next 下一步

* [Create and manage users](create-and-manage-users.md) 创建和管理用户
* [Create and manage teams](create-and-manage-teams.md) 创建和管理团队
