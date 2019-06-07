---
layout: post
author: Jonathan Frappier
title: Create a Deployment Environment - Application Services Series Part 2
permalink: /:title/
modified: 2014-11-23
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac, app services]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---

In my last post we created the Cloud Provider, now we need to setup a Deployment Environment.  We are really setting up logical constructs here to map to to services and resources we already have.  So far we mapped a Cloud Provider to vRealize Automation Center and to our business group.  Now we are going to create a deployment environment for Application Services that maps to the cloud provider we created, that maps to vRealize resources... If you are not already, log in as Luke and perform the following.
<ul>
	<li>Click on the Cloud Provider pull down menu in the top right corner (short aside, that menu name will change to show the context you are currently working in so if you logged out this may be different) and select Deployment Environments</li>
	<li>Click the Create A Deployment Environment button</li>
	<li>Provide a name</li>
	<li>Select a Cloud Provider from the pull down menu</li>
	<li>Click the Select button to select a reservation policy; click OK then Save</li>
</ul>
You screen should look similar to what is pictured below.  Since Application Services is now part of vRealize Automation, most of the work we are doing here will map to what has already been configured in vRealize Automation.  Next we need to create a logical template.

<img src="/images/fulls/apps-cloud-provider.png" class="fit image">