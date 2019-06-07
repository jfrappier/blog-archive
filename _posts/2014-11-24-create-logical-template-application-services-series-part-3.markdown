---
layout: post
author: Jonathan Frappier
title: Create a Logical Template - Application Services Series Part 3
permalink: /:title/
modified: 2014-11-24
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac, apps, app services, application services]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Enabling Disaster Recovery with Ansible", 
    "DevOps IRL Boston VMUG Usercon with @szelechoski"
]
---
[display-posts category="Application Services Lab" display-posts posts_per_page="15"]

We are making good progress with Application Services, up next is creating a logical template, which will be part of our deployment environment, which is part of our cloud provider, which is ultimately vRealize Automation Center, which runs on vCenter!  If you are not logged into Application Services, log in as luke now (assuming your usernames are the same as mine).
<ul>
	<li>Click on the Deployment Environment pull down (recall that menu changes context) and click on Chose Logical Templates</li>
	<li>Click the Logical Template that matches your virtual machine template.  My CentOS template is 6.6 but I have selected CentOS6.4 64-bit</li>
	<li>Click on the box for the logical template version 1.0.0</li>
	<li>In the upper right corner click the Edit button</li>
	<li>Click on the green plus icon next to Cloud Template Mapping</li>
	<li>In the Cloud Provider pull down, select the cloud provider we previously created</li>
	<li>In the Cloud Template pull down, select the cloud template we previously crated</li>
</ul>
Your screen should look similar to what is below.  Click the Save button.  Much like we have done before, the logical template simply combines other items and groups them together - in this case the vSphere template we created, which is a vRealize Automation blueprint, which is added as a template to the Application Services Cloud Provider which is part of a deployment environment.

<img src="/images/fulls/apps-logical-template.png" class="fit image">