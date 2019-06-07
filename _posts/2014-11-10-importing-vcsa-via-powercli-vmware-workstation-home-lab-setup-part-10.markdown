---
layout: post
author: Jonathan Frappier
title: Importing VCSA via PowerCLI - VMware Workstation Home Lab Setup Part 10
permalink: /:title/
modified: 2014-11-10
category: vsphere
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
In the last post we looked quickly at importing the vCenter Server Appliance through the vSphere Client, however its high time we introduce PowerCLI.  PowerCLI is a set of PowerShell cmdlets to manage your VMware environments (vSphere, vCloud Director and View) and is quite powerful.  So powerful in fact that this is going to be a pretty short post, the 7 bullet points needed just to import the OVF through the vSphere Client is now a single command!

Launch PowerCLI as administrator and run the following to log in:
<pre>connect-viserver 192.168.6.11</pre>
Log in as root when prompted.

Now either change to the directory the OVF is located in or make note of the location for the cmdlet:
<pre>Import-VApp -source .\VMware-vCenter-Server-Appliance-5.5.x.xxxxx-xxxxxxx_OVF.ova -VMhost 192.168.6.11 -DiskStorageFormat thin -name vxprt-vc01</pre>
Below you can see it in console as well as it happening if you were to log into the vSphere Client

<img src="/images/fulls/powercli-import-vapp.png" class="fit image">

<img src="/images/fulls/vsphere-deploying-via-powercli.png" class="fit image">

1 command - how do you not love PowerCLI?  You can see all of the options available in the <a href="https://www.vmware.com/support/developer/PowerCLI/PowerCLI51/html/Import-VApp.html" target="_blank">Import-VApp cmdlet in the PowerCLI documentation</a>.  Now having shown you those options, I am actually going to run the VCSA in VMware Workstation, simply click File &gt;&gt; Open, select the OVF then provide a name and location.  Power on the VCSA and see my post <a title="Installing the vCenter Server Appliance 5.5.0b #VCSA" href="http://www.virtxpert.com/installing-vcenter-server-appliance-5-5-0b/">here about configuring i</a>t.