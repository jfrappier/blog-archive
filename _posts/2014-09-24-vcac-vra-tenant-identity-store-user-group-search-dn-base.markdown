---
layout: post
author: Jonathan Frappier
title: vCAC vRA Tenant Identity Store - User and Group search DN base
permalink: /:title/
modified: 2014-09-24
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac, identitiy store]
share: true
related: [
    "vSphere SSO or vRA Identity Appliance - vRealize Automation Series Part 1", 
    "Configure the vRealize Automation Appliance - vRealize Automation Series Part 3", 
    "Create an Application Blueprint - Application Services Series Part 4"
]
---
I am in a course this week and a question came up about how to configure the Group and User search base DN and its effect on access within vCAC / vRA.  Ultimately permission will be granted as a combination of both fields.  First and foremost when configuring vCloud Automation Center for vRealize Automation tenants this will control which users or groups you can assign tenant administrator or infrastructure administrator roles.  Let's look at some examples; if my domain is test.lab and I set my User and Group search base DN to dc=test,dc=lab I will be able to assign either of those roles to any user or group in the entire Active Directory, regardless of what organization unit or container they may be in.  Easy enough, but that starts to open things up pretty wide.

<img src="/images/fulls/vcac-is-tld.png" class="fit image">

In the real world I am likely to have an OU for groups or in a large enough AD groups spread across multiple OUs so you'll need to consider your AD structure to set the base DN appropriately.  For example if you have ou=groups,dc=test,dc=lab as your Group search base DN but you have some groups created in ou=sales,dc=test,dc=lab you will not be able to assign permissions to the groups in the Sales OU.

You may have noticed from the screenshot that only the Group search base DN is required, personally I prefer to assign permissions to groups so that is great - what happens if I leave the User search base DN empty and set a more restrictive Group search base DN such as ou=groups,dc=test,dc=lab like so?

<img src="/images/fulls/vcac-is-groupou-only.png" class="fit image">

In this scenario, <strong>BOTH</strong> the user account, and group would need to be in the same OU; not something I'd typically do.

<img src="/images/fulls/vcac-incorrect-username.png" class="fit image">

What I am likely to do given this scenario is to set my user search base more broadly and be more restrictive on my Group search base so that only specific groups can be assigned permission in vCAC - let's look at that scenario.  I have created an OU called org-users with a sub-OU called sales and an OU called org-groups.  I set the Group search base DN to ou=org-groups,dc=test,dc=lab and the User search base DN to ou=org-users,dc=test,dc=lab.

In the org-groups OU I have a group called vcac-admins.  In the Sales OU (which is under org-users) I have a user called test and a group called sales.  Since Group search base DN is set to org-groups I cannot assign the sales group either tenant administrator or infrastructure administrator roles.  I can however assign my vcac-admins group permission since that falls within the Group search base.

<img src="/images/fulls/vcac-permissions.jpg" class="fit image">

Now, so long as my test user falls within the scope of the User search base - in this case the sub-OU called Sales under org-users and is a member of the vcac-admins group (even though the group and user are not in the same OU like I would need if I did not define the User base search DN field.)

<img src="/images/fulls/test-lab-ad-org.png" class="fit image">

Now I can log in with the expected permissions.  In short, if you leave the User search base DN blank, your user accounts need to fall within the same scope as the Group search base DN.

<img src="/images/fulls/test-user-logged-in.png" class="fit image">