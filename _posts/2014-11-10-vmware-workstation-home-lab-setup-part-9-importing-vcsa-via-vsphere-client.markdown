---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 9 – Importing VCSA via vSphere Client
permalink: /:title/
modified: 2014-11-10
category: vra
tags: [vmware, vsphere, vcenter, home lab, vcsa]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---

All right - ESXi hosts built, datastores created (on at least 1 ESXi hosts) so lets import the vCenter Virtual Appliance.  The VCSA should be a bit lighter weight for our home lab that vCenter on Windows + SQL.  Before getting started, make sure you have download the VCSA from VMware and placed it in a location accessible to the vSphere Client.
<ul>
	<li>Launch the vSphere Client and connect to one of the ESXi hosts you added the local datastores to, in my case vxprt-esxi01</li>
	<li>Click on File &gt;&gt; Deploy OVF Template</li>
	<li>Browse to the location of the VCSA you downloaded from VMware and click Next, then Next again</li>
	<li>Name the VCSA, I'll keep to my naming conventions so vxprt-vc01 and click Next</li>
	<li>Select the storage you wish to place the VM on and click Next</li>
	<li>Select Thin Provision and click Next</li>
	<li>Here you could click finish, I am not as I also want to demonstrate importing the OVF using PowerCLI so I have clicked Cancel</li>
</ul>
<img src="/images/fulls/deploy-vcsa-ovf.png" class="fit image">

If you are not interested in deploying via PowerCLI, go ahead and click finish, the OVF will be imported.  The rest of the setup for the VCSA is something I have written about in the past, <a title="Installing the vCenter Server Appliance 5.5.0b #VCSA" href="http://www.virtxpert.com/installing-vcenter-server-appliance-5-5-0b/">Installing the vCenter Server Appliance 5.5</a> which is also one of the more popular pages people find my site on from Google.  Have a read over those steps and we will pickup the rest of the setup in part 11.  For those interested, stay tuned for how to import the OVF via PowerCLI next.