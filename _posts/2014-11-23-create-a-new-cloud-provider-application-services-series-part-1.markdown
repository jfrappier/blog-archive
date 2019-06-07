---
layout: post
author: Jonathan Frappier
title: Create a New Cloud Provider - Application Services Series Part 1
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

VMware Application Services (formerly Application Director) is now deployed, but we need to do a bit more integration with vRealize Automation / vCloud Automation Center so we can publish Application Services blueprints to the vRealize Automation catalog.  First we need to define a cloud provider;
<ul>
	<li>While logged in as Luke, the user we gave all of the Application Services roles to, click on the Applications pull down menu and select Cloud Providers</li>
	<li>Click the Create Cloud Provider button/box</li>
	<li>Click the Cloud Provider Type pull down and notice what options are available - vCloud 5.x, vCAC and EC2.  What about vCloud Air - can we use that?  If you said yes you are correct because vCloud Air is based on vCloud Director.</li>
	<li>Enter the information like so (note some of the boxes appear "greyed out" - they are not, just a poor choice for background colors) and click the Validate Connection button</li>
</ul>
<img src="/images/fulls/apps-validate-cloud-provider.png" class="fit image">
<ul>
	<li>Notice that you have to use an upper case domain, I'm curious as to why but in any case its the only way that worked for me</li>
	<li>Next in the lower half of the screen (not pictured above) in the templates section, click the green plus icon</li>
	<li>You should see the CentOS-Template catalog item we previously published in the vRealize Automation catalog; click the check mark next to the desired template and click OK</li>
	<li>Click the Save button in the upper right hand corner</li>
</ul>
We now have the first step in setting up Application Services complete, up next we will create a Deployment Enviornment