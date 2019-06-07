---
layout: post
author: Jonathan Frappier
title: Creating Entitlements - vRealize Automation Series Part 15
permalink: /:title/
modified: 2014-11-22
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---


Home stretch, 15 posts and we are about to see our first catalog item published!  Lets get going and create the entitlement which is how we define what can be done in vRealize Automation / vCloud Automation Center
<ul>
	<li>Log in as tenantadmin</li>
	<li>Click Administration &gt;&gt; Catalog Management &gt;&gt; Entitlements</li>
	<li>Click the Add button and fill in the information as follows</li>
</ul>
<img src="/images/fulls/vra-entitlement.png" class="fit image">
<ul>
	<li>Click the Next button</li>
	<li>Click the plus sign next to Entitled Services, select Clone Linux Template and click OK</li>
	<li>Click the plus sign next to Entitled Catalog items, select CentOS template and click OK</li>
	<li>Click the plus sign next to Entitled Actions, Select Machine from the pull down and chose all of the items, Select Virtual Machine from the pull down and select Destroy; click OK</li>
	<li>Click the Add button</li>
</ul>
Log out as tenantadmin and log back in as luke, you should now see your vSphere template, which is now a vRealize Automation / vCloud Automation Center blueprint published!

<img src="/images/fulls/hooray.png" class="fit image">