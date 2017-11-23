---
title: Create and manage organizations �����͹�����֯
description: Learn how to set up organizations to enforce security in Docker Trusted
  Registry.
  �˽�����½���֯�Լ�ǿDTR��ȫ
keywords: registry, security, permissions, organizations
ע�ᣬ��ȫ��Ȩ�ޣ���֯
---

When a user creates a repository, only that user has permissions to make changes
to the repository.

���û������洢��ʱ��ֻ�и��û���Ȩ���и��ĵ��洢�⡣

For team workflows, where multiple users have permissions to manage a set of
common repositories, create an organization. By default, DTR has one
organization called 'docker-datacenter', that is shared between DTR and UCP.

���ڶ���û���Ȩ����һ�鹫���洢����Ŷӹ������̣��봴��һ����֯�� Ĭ������£�DTR��һ����Ϊ��docker-datacenter������֯����DTR��UCP֮�乲��

To create a new organization, navigate to the **DTR web UI**, and go to the
**Organizations** page.

������֯����鵽**DTR web UI**�½ڣ��������**Organizations**ҳ��

![](../../images/create-and-manage-orgs-1.png){: .with-border}

Click the **New organization** button, and choose a meaningful name for the
organization.
���**New organization**��ť����������

![](../../images/create-and-manage-orgs-2.png){: .with-border}

Repositories owned by this organization will contain the organization name, so
to pull an image from that repository, you'll use:

����֯��ӵ�еĴ洢�⽫������֯������ˣ�Ϊ����ȡ�洢��ľ�����Ҫʹ�ã�

```bash
$ docker pull <dtr-domain-name>/<organization>/<repository>:<tag>
```

Click **Save** to create the organization, and then **click the organization**
to define which users are allowed to manage this
organization. These users will be able to edit the organization settings, edit
all repositories owned by the organization, and define the user permissions for
this organization.

���**Save**�Դ�����֯��Ȼ����**click the organization**������Щ�û�����������֯����Щ�û������Ա༭��֯�����ã��༭����֯���еĴ洢�⣬������ɷ��ʸ���֯���û���

For this, click the **Add user** button, **select the users** that you want to
grant permissions to manage the organization, and click
**Save**. Then change their permissions from 'Member' to **Admin**.

�Դˣ����**Add user** ��ť��ѡ��һ����Ҫ�������Ȩ�޵��û������**Save**��ť�����ţ�����Щ�û���Ȩ�޴�MemberΪ**Admin**��

![](../../images/create-and-manage-orgs-3.png){: .with-border}

## Where to go next ��һ��

* [Create and manage users](create-and-manage-users.md) �����͹����û�
* [Create and manage teams](create-and-manage-teams.md) �����͹����Ŷ�
