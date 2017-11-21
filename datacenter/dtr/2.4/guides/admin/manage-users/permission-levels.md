---
title: Permission levels in DTR
description: Learn about the permission levels available in Docker Trusted Registry.了解Docker信任注册器的权限级别
keywords: registry, security, permissions 注册 安全 权限
---

Docker Trusted Registry allows you to define fine-grain permissions over image
repositories.

Docker Trusted Registry允许你定义镜像的细粒度权限库。

## Administrator users 管理员用户

Users are shared across UCP and DTR. When you create a new user in Docker
Universal Control Plane, that user becomes available in DTR and vice versa.
When you create an administrator user in DTR, the user has permissions to:

用户在UCP和DTR之间共享。 当您在Docker Universal Control Plane中创建新用户时，该用户将变为DTR中的可用状态，反之亦然。
在DTR中创建管理员用户时，用户有权执行以下操作：

* Manage users across UCP and DTR, 管理UCP和DRT之间的用户
* Manage DTR repositories and settings, 管理DRT存储库和设置
* Manage UCP resources and settings. 管理UCP的资源集合设置

## Team permission levels 团队权限级别


Teams allow you to define the permissions a set of user has for a set of
repositories. Three permission levels are available:

团队允许您定义一组用户对一组存储库的权限。 三个权限级别可用：（read read-write admin）


| Repository operation  | read | read-write | admin |
|:----------------------|:----:|:----------:|:-----:|
| View/ browse          |  x   |     x      |   x   |
| Pull                  |  x   |     x      |   x   |
| Push                  |      |     x      |   x   |
| Start a scan          |      |     x      |   x   |
| Delete tags           |      |     x      |   x   |
| Edit description      |      |            |   x   |
| Set public or private |      |            |   x   |
| Manage user access    |      |            |   x   |
| Delete repository     |      |            |   x   |

Team permissions are additive. When a user is a member of multiple teams, they
have the highest permission level defined by those teams.

团队的权限是叠加的。 当某用户是多个团队的成员时，他们具有由这些团队定义的最高权限级别。


## Overall permissions 权限总概

Here's an overview of the permission levels available in DTR:

* Anonymous users: Can search and pull public repositories.
* Users: Can search and pull public repos, and create and manage their own
repositories.
* Team member: Everything a user can do, plus the permissions granted by the teams the user is member of.
* Team admin: Everything a team member can do, and can also add members to the team.
* Organization admin: Everything a team admin can do, can create new teams, and add members to the organization.
* Admin: Can manage anything across UCP and DTR.

以下是DTR中可用权限级别的概述：

* 匿名用户：可以搜索和拉动（pull）公共仓库。
* 用户：可以搜索和拉回公共回购，并创建和管理自己的库。
* 团队成员：可以做普通用户的所有事情，以及用户所在团队授予的权限。
* 团队管理员：团队成员可以做的所有事情，也可以将成员添加到团队中。
* 组织管理员：团队管理员可以执行的任何操作，可以创建新的团队，并将成员添加到组织中。
* 管理员：可以管理任何UCP和DTR。

## Where to go next 下一步

* [Authentication and authorization](index.md)
