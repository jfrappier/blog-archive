---
layout: post
author: Jonathan Frappier
title: Deploying the VMware vCenter 5.5 Appliance VCSA
permalink: /:title/
modified: 2013-09-23
category: vcsa
tags: [vmware, vsphere, vcenter, vcsa]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
<strong>**Update:  There is a newer version of this post at <a href="http://www.virtxpert.com/installing-vcenter-server-appliance-5-5-0b/">http://www.virtxpert.com/installing-vcenter-server-appliance-5-5-0b/</a> - the steps have changed a bit**</strong>

As you may be aware, the VMware vCenter 5.5 appliance now supports somewhere between 300 and 500 hosts; I know quite  range but I am still finding contradicting documentation.  Anyways, worst case scenario it will support up to 300 hosts or 3000 VMs - more than enough for a lot of environments.  In this post I will review the installation steps for the appliance.
<ul>
	<li>Download and import the OVF/OVA.  If you haven't done an OVA/OVF import just have your network settings handy and follow the prompts.</li>
	<li>Make sure to set your host name as a FQDN (so host.doman.tld)</li>
	<li>**The process would NOT allow me to use an over provisioned datastore, even though I would be thin provisioning so make sure you have 125GB free**</li>
	<li>When the deployment finishes, power on the VM (or tick the power on check box at the end of the deployment wizard).</li>
	<li>**Another apparent problem, the network settings provided during the OVA/OVF deployment wizard didn't take, it booted up configured for DHCP**</li>
	<li>Navigate to the URL on the console:  https://your.ip.address:5480 (Default username  and password is root / vmware)</li>
	<li>Accept the license agreement.</li>
	<li>If, like me you need to change your network settings, you will be told to cancel the wizard (seems a little lazy to me VMware)</li>
	<li>Cancel, go to the network tab and configure your network settings, dont forget to enter the FQDN and create a DNS record for the host name if you plan on using AD.</li>
	<li>Navigate to the new IP you just set (dont forget :5480)</li>
	<li>Click the 'Launch' button the right to restart the setup wizard.</li>
	<li>I opted to use the custom settings, selected the embeded database and embedded SSO</li>
	<li>Enter the password for the administrator@vsphere.local SSO user</li>
	<li>Click start, if all is well in your universe you should get green checkmarks for AD, database and SSO configuration then finally starting services</li>
</ul>
You should now be able to log in as administrator@vsphere.local and configure your AD groups/users for access to vCenter, add host and other virtualization goodness.  If you did not set an FQDN during setup, you will not be able to configure AD authentication, so make sure to verify that setting.

<strong>Conclusion</strong>

<strong></strong>After a bumpy start with the VM not taking the network settings from the OVF/OVA deployment wizard, deploying the appliance was quite smooth.  Remember to give your host a FQDN and create DNS records for it and get on to your 5.5 upgrade projects!