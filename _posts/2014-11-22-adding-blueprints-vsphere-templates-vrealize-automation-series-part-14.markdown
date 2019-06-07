---
layout: post
author: Jonathan Frappier
title: Adding Blueprints from vSphere Templates - vRealize Automation Series Part 14
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

We've got our reservations done, but so far we haven't created any catalog items for our Georgia and Alderaan employees to actually request.  One of the simplest things to publish in the vRealize Automation / vCloud Automation Center catalog are virtual machine blueprints; which are created from vSphere templates.  With our virtual machine converted to a template in vCenter, we should be ready to go.
<ul>
	<li>Log into vRealize Automation as iaasadmin</li>
	<li>Click on Infrastructure &gt;&gt; Blueprints &gt;&gt; Machine Prefixes</li>
	<li>Create a Prefix like we did for our business groups, call this one nix</li>
	<li>Click on Infrastructure &gt;&gt; Compute Resources &gt;&gt; Compute Resources</li>
	<li>Hover over cl01 &gt;&gt; Data Collection</li>
	<li>Wait a few moments and click the Refresh button at the bottom of the screen; status should be Succeeded</li>
	<li>Under Inventory, click Request Now</li>
	<li>Log out and log back in as tenantadmin</li>
	<li>Click on Infrastructure &gt;&gt; Blueprints &gt;&gt; Blueprints</li>
	<li>Hover over New Blueprint &gt;&gt; Virtual &gt;&gt; vSphere (vCenter)</li>
	<li>Fill in the build information similar to below</li>
</ul>
<img src="/images/fulls/vra-blueprint.png" class="fit image">
<ul>
	<li>Click the Build Information Tab</li>
	<li>Change Action to Clone</li>
	<li>Click the ellipse next to Clone From and select your linux template and click the OK button</li>
	<li>For testing in the lab, leave everything else as is and click the OK button at the bottom of the page</li>
	<li>Hover over the new Blueprint, click on Publish then click OK</li>
	<li>Navigate to Administration &gt;&gt; Catalog Management &gt;&gt;Services</li>
	<li>Click the Add button, name it Clone Linux Template, set it to Active, and click the Add button</li>
	<li>Highlight the new service and click the Manage Catalog Items button</li>
	<li>Click the green + icon, select CentOS-Template and click Add</li>
	<li>Click Close</li>
</ul>
Almost there I promise, now that the blueprint, service and catalog item is created, we just need to provide entitlements so our users can see it!