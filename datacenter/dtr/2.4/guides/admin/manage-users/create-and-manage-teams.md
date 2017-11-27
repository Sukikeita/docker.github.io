---
title: Create and manage teams in DTR 在DTR中创建和管理团队
description: Learn how to manage teams to enforce fine-grain access control in Docker 了解如何管理团队，以便在Docker中实施细粒度的访问控制
  Trusted Registry. 信任的注册机制
keywords: registry, security, permissions, teams 注册机制 安全 权限 团队
---

You can extend a user's default permissions by granting them individual
permissions in other image repositories, by adding the user to a team. A team
defines the permissions a set of users have for a set of repositories.

通过将用户添加到团队中，您可以通过在其他镜像存储库中授予其个人权限（individual permission）来扩展用户的默认权限。 一个团队定义了一组用户对一组存储库的权限。

To create a new team, go to the **DTR web UI**, and navigate to the
**Organizations** page.

要创建一个新的团队，请转到**DTR web UI**，然后导航到**Organizations**页面。 

Then **click the organization** where you want to create the team. In this
example, we'll create the 'billing' team under the 'whale' organization.

然后点击**click the organization**。 在这个例子中，我们将在“whale”组织下创建“billing”团队。

![](../../images/create-and-manage-teams-1.png){: .with-border}

Click '**+**' to create a new team, and give it a name.

![](../../images/create-and-manage-teams-2.png){: .with-border}

## Add users to a team 添加用户到团队

Once you have created a team, **click the team** name, to manage its settings.
The first thing we need to do is add users to the team. Click the **Add user**
button and add users to the team.

一旦你创建了一个团队，点击**团队名称**来管理其设置。 我们需要做的第一件事就是将用户添加到团队中。 点击**添加用户**按钮并添加用户到团队。

![](../../images/create-and-manage-teams-3.png){: .with-border}

## Manage team permissions

The next step is to define the permissions this team has for a set of
repositories. Navigate to the **Repositories** tab, and click the
**Add repository** button.

![](../../images/create-and-manage-teams-4.png){: .with-border}

Choose the repositories this team has access to, and what permission levels the
team members have.

![](../../images/create-and-manage-teams-5.png){: .with-border}

There are three permission levels available:

| Permission level | Description                                                      |
|:-----------------|:-----------------------------------------------------------------|
| Read only        | View repository and pull images.                                 |
| Read & Write     | View repository, pull and push images.                           |
| Admin            | Manage repository and change its settings, pull and push images. |

## Where to go next

* [Create and manage users](create-and-manage-users.md)
* [Permission levels](permission-levels.md)
