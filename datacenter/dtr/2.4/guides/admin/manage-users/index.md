---
title: Authentication and authorization in DTR
description: Learn about the permission levels available on Docker Trusted Registry.
keywords: registry, security, permissions, users
---

With DTR you get to control which users have access to your image repositories.

ʹ��DTR�������Կ�����Щ�û����Է������ľ���洢�⡣

By default, anonymous users can only pull images from public repositories.
They can't create new repositories or push to existing ones.
You can then grant permissions to enforce fine-grained access control to image
repositories. For that:
Ĭ������£������û�ֻ�ܴӹ����洢������ȡ����
���ǲ��ܴ����µĴ洢���push�����еĴ洢�⡣
Ȼ������������Ȩ�ޣ��ԶԾ���ִ��ϸ���ȵķ��ʿ��ƿ⡣ �Դ�����ԣ�

* Start by creating a user. ���ȴ���һ����ͨ�û�

    Users are shared across UCP and DTR. When you create a new user in
    Docker Universal Control Plane, that user becomes available in DTR and vice
    versa. Registered users can create and manage their own repositories.
    �û���UCP��DTR֮�乲�� ������Docker Universal Control Plane�д������û�ʱ�����û�����ΪDTR�еĿ���״̬����֮��Ȼ�� ע���û����Դ����͹����Լ��Ĵ洢�⡣
    

    You can also integrate with an LDAP service to manage users from a single
    place.
    
    ��Ҳ������LDAP���񼯳��Դ�һ���ط������û���

* Extend the permissions by adding the user to a team. ͨ�����û���ӵ��Ŷ�����չȨ�ޡ�

    To extend a user's permission and manage their permissions over repositories,
    you add the user to a team.
    A team defines the permissions users have for a set of repositories.
    
    Ҫ��չ�û���Ȩ�޲�ͨ���洢�������Ȩ�ޣ��뽫���û���ӵ��Ŷ��С� �ŶӶ������û���һ��洢���Ȩ�ޡ�
    


## Organizations and teams ��֯���Ŷ�

When a user creates a repository, only that user can make changes to the
repository settings, and push new images to it.

���û������洢��ʱ��ֻ�и��û����Ը��Ĵ洢�����ã������¾������͵��ô洢�����á�


Organizations take permission management one step further, since they allow
multiple users to own and manage a common set of repositories. This
is useful when implementing team workflows. With organizations you can
delegate the management of a set of repositories and user permissions to the
organization administrators.

��֯��һ����ȡȨ�޹�����Ϊ�����������û�ӵ�к͹���һ��ͨ�õĴ洢�⡣ ����ʵʩ�Ŷӹ�������ʱ�����á� ͨ����֯�������Խ�һ��洢����û�Ȩ�޵Ĺ���ί�ɸ���֯����Ա��


An organization owns a set of repositories, and defines a set of teams. With
teams you can define fine-grain permissions that a team of
user has for a set of repositories.

��֯ӵ��һ��洢�⣬������һ��С�顣 ͨ���Ŷӣ������Զ���һ���û���һ��洢��ľ�ϸȨ�ޡ�

![](../../images/authentication-authorization-1.svg)

In this example, the 'Whale' organization has three repositories and two teams:

����������У���Whale����֯�������ֿ�������Ŷӣ�


* Members of the blog team can only see and pull images from the whale/java
repository, blog�Ŷӵĳ�Աֻ�ܿ����ʹ�whale/java������ȡ����

* Members of the billing team can manage the whale/golang repository, and push
and pull images from the whale/java repository. billingС��ĳ�Ա���Թ���whale/golang�洢�⣬����whale/java�洢�����ơ�������


## Where to go next

* [Create and manage users](create-and-manage-users.md) �����͹����û�
