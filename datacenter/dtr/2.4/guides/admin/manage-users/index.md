---
title: Authentication and authorization in DTR
description: Learn about the permission levels available on Docker Trusted Registry.
keywords: registry, security, permissions, users
---

With DTR you get to control which users have access to your image repositories.

使用DTR，您可以控制哪些用户可以访问您的镜像存储库。

By default, anonymous users can only pull images from public repositories.
They can't create new repositories or push to existing ones.
You can then grant permissions to enforce fine-grained access control to image
repositories. For that:
默认情况下，匿名用户只能从公共存储库中拉取镜像。
他们不能创建新的存储库或push到现有的存储库。
然后，您可以授予权限，以对镜像执行细粒度的访问控制库。 对此你可以：

* Start by creating a user. 首先创建一个普通用户

    Users are shared across UCP and DTR. When you create a new user in
    Docker Universal Control Plane, that user becomes available in DTR and vice
    versa. Registered users can create and manage their own repositories.
    用户在UCP和DTR之间共享。 当您在Docker Universal Control Plane中创建新用户时，该用户将变为DTR中的可用状态，反之亦然。 注册用户可以创建和管理自己的存储库。
    

    You can also integrate with an LDAP service to manage users from a single
    place.
    
    您也可以与LDAP服务集成以从一个地方管理用户。

* Extend the permissions by adding the user to a team. 通过将用户添加到团队来扩展权限。

    To extend a user's permission and manage their permissions over repositories,
    you add the user to a team.
    A team defines the permissions users have for a set of repositories.
    
    要扩展用户的权限并通过存储库管理其权限，请将该用户添加到团队中。 团队定义了用户对一组存储库的权限。
    


## Organizations and teams 组织和团队

When a user creates a repository, only that user can make changes to the
repository settings, and push new images to it.

当用户创建存储库时，只有该用户可以更改存储库设置，并将新镜像推送到该存储库设置。


Organizations take permission management one step further, since they allow
multiple users to own and manage a common set of repositories. This
is useful when implementing team workflows. With organizations you can
delegate the management of a set of repositories and user permissions to the
organization administrators.

组织进一步采取权限管理，因为它们允许多个用户拥有和管理一组通用的存储库。 这在实施团队工作流程时很有用。 通过组织，您可以将一组存储库和用户权限的管理委派给组织管理员。


An organization owns a set of repositories, and defines a set of teams. With
teams you can define fine-grain permissions that a team of
user has for a set of repositories.

组织拥有一组存储库，并定义一组小组。 通过团队，您可以定义一组用户对一组存储库的精细权限。

![](../../images/authentication-authorization-1.svg)

In this example, the 'Whale' organization has three repositories and two teams:

在这个例子中，“Whale”组织有三个仓库和两个团队：


* Members of the blog team can only see and pull images from the whale/java
repository, blog团队的成员只能看到和从whale/java库中提取镜像，

* Members of the billing team can manage the whale/golang repository, and push
and pull images from the whale/java repository. billing小组的成员可以管理whale/golang存储库，并从whale/java存储库中推、拉镜像。


## Where to go next

* [Create and manage users](create-and-manage-users.md) 创建和管理用户
