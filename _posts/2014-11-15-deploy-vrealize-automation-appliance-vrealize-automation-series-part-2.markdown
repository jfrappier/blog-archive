---
layout: post
author: Jonathan Frappier
title: Deploy the vRealize Automation Appliance - vRealize Automation Series Part 2
permalink: /:title/
modified: 2014-11-15
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
With the decision made to use vSphere SSO for vCloud Automation Center / vRealize Automation, it's time to deploy the vRealize Appliance.  The appliance comes as, well an appliance as the name suggests and is a straight forward OVF deployment.  One thing to keep in mind, at least as of 6.0 it was very difficult to change the network settings that were supplied through the OVF properties during the deployment.  If you fat finger something here just punt and redeploy - something I'm likely to hold on to for a while.

Also, we don't really want to skimp on vCloud Automation Center / vRealize Automation resources, to that end I have increased the amount of memory in both of my ESXi hosts to 8GB instead of 4 to ensure the appliance can access the resource it needs.

Since we are going to start using the Web Client to build virtual machines and deploy OVFs, now would be the time to create an entry in your host file so you can access the web client via FQDN, otherwise you will not be able to deploy OVFs for example, like we are about to do!  You should also add a host file entry for your vCloud Automation Center / vRealize Automation appliance.  In addition, I also had to do this from the domain controller behind the NAT, doing it from my home computer running workstation kept having errors.
<ul>
	<li>Log into the vSphere Web Client</li>
	<li>Click on vCenter &gt;&gt; Hosts and Clusters</li>
	<li>Right click your cluster and select deploy OVF template (if you hadn't already you will be prompted to download the client integration plug-in, do so and come back when finished)</li>
	<li>Browse to the location where you've download the OVF, click Next, Next, Accept, Next (unless you like reading EULA's then go ahead and do that)</li>
	<li>Name and select the datacenter or folder you wish to place the virtual machine in, click Next</li>
	<li>Select the datastore; I want to give my Synology a roll so I'll be selecting my gold datastore (let's see how that works out), once the datastore is selected chose Thin</li>
	<li>Select the port group for your virtual machine traffic; I have a vm port group on my VDS I will be using, click Next</li>
	<li>The customize template screen is where you really need to pay attention!</li>
	<li>Enter the root password and the fqdn of the appliance, in my case vxprt-vcac01.vxprt.local</li>
	<li>Expand Networking Properties and fill in as appropriate for your environment; click the finish button</li>
	<li>Do NOT power on the virtual machine yet, first, we have to create a DNS record in AD for the appliance</li>
	<li>If you are not already, log into the DC, open the start menu &gt;&gt; Administrative Tools &gt;&gt; DNS</li>
	<li>Right click on the forward lookup zone for your domain, select New Host (A or AAA...)</li>
	<li>Enter the hostname and IP address used during the OVF deployment, in my case vxprt-vcac01 and 192.168.6.20</li>
	<li>Make sure that Create associated pointer (PTR) record is selected and click the Add Host button</li>
	<li>Power on the virtual machine, once it has powered on navigate to the FQDN of your appliance on port 5480, for example https://vxprt-vcac01.vxprt.local:5480</li>
</ul>
You can now log in and start to configure the vCloud Automation Center / vRealize Automation appliance - which as you may have guessed will be my next blog post!

<img src="/images/fulls/vcac-appliance-vami-login.png" class="fit image">