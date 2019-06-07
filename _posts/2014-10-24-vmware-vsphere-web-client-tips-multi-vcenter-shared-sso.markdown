---
layout: post
author: Jonathan Frappier
title: VMware vSphere Web Client Tips - Multi vCenter Shared SSO
permalink: /:title/
modified: 2014-10-24
category: vsphere
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
A great feature of the vSphere Web Client comes when you have two separate vCenter servers that share a common Single Sign-on  (SSO) server.  In this scenario you can see all vCenter servers connected to the shared SSO server - logging into the vSphere Web Client for either vCenter (or having a single web server running the Web Client bits) you can see any/all vCenter servers using the shared SSO server.

As you can see in the screenshot below, I have VC1 and VC2, along with all of the resources you would expect to have access to (<a title="VMware vSphere Web Client Tips – Love Related Objects Tab" href="http://www.virtxpert.com/vmware-vsphere-web-client-tips-love-related-objects-tab/">including the awesome related objects tab</a>) but I only have to log into one client!
<img src="/images/fulls/vsphere-web-client-shared-sso-multiple-vcenter.png" class="fit image">