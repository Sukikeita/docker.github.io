---
title: Permission levels in DTR
description: Learn about the permission levels available in Docker Trusted Registry.�˽�Docker����ע������Ȩ�޼���
keywords: registry, security, permissions ע�� ��ȫ Ȩ��
---

Docker Trusted Registry allows you to define fine-grain permissions over image
repositories.

Docker Trusted Registry�����㶨�徵���ϸ����Ȩ�޿⡣

## Administrator users ����Ա�û�

Users are shared across UCP and DTR. When you create a new user in Docker
Universal Control Plane, that user becomes available in DTR and vice versa.
When you create an administrator user in DTR, the user has permissions to:

�û���UCP��DTR֮�乲�� ������Docker Universal Control Plane�д������û�ʱ�����û�����ΪDTR�еĿ���״̬����֮��Ȼ��
��DTR�д�������Ա�û�ʱ���û���Ȩִ�����²�����

* Manage users across UCP and DTR, ����UCP��DRT֮����û�
* Manage DTR repositories and settings, ����DRT�洢�������
* Manage UCP resources and settings. ����UCP����Դ��������

## Team permission levels �Ŷ�Ȩ�޼���


Teams allow you to define the permissions a set of user has for a set of
repositories. Three permission levels are available:

�Ŷ�����������һ���û���һ��洢���Ȩ�ޡ� ����Ȩ�޼�����ã���read read-write admin��


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

�Ŷӵ�Ȩ���ǵ��ӵġ� ��ĳ�û��Ƕ���Ŷӵĳ�Աʱ�����Ǿ�������Щ�ŶӶ�������Ȩ�޼���


## Overall permissions Ȩ���ܸ�

Here's an overview of the permission levels available in DTR:

* Anonymous users: Can search and pull public repositories.
* Users: Can search and pull public repos, and create and manage their own
repositories.
* Team member: Everything a user can do, plus the permissions granted by the teams the user is member of.
* Team admin: Everything a team member can do, and can also add members to the team.
* Organization admin: Everything a team admin can do, can create new teams, and add members to the organization.
* Admin: Can manage anything across UCP and DTR.

������DTR�п���Ȩ�޼���ĸ�����

* �����û�������������������pull�������ֿ⡣
* �û����������������ع����ع����������͹����Լ��Ŀ⡣
* �Ŷӳ�Ա����������ͨ�û����������飬�Լ��û������Ŷ������Ȩ�ޡ�
* �Ŷӹ���Ա���Ŷӳ�Ա���������������飬Ҳ���Խ���Ա��ӵ��Ŷ��С�
* ��֯����Ա���Ŷӹ���Ա����ִ�е��κβ��������Դ����µ��Ŷӣ�������Ա��ӵ���֯�С�
* ����Ա�����Թ����κ�UCP��DTR��

## Where to go next ��һ��

* [Authentication and authorization](index.md)
