---
layout: post
author: Jonathan Frappier
title: vCenter Setup - VMware Workstation Home Lab Setup Part 11
permalink: /:title/
modified: 2014-11-11
category: vsphere
tags: [vmware, vsphere, vcenter, home lab]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career.", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
The home lab is getting close!  With the vCenter Server Appliance deployed and basic configuration done, its time to get vCenter setup - AD permissions, Data Center, Cluster and adding hosts to the cluster.  While there are only 2 hosts so far in the home lab, its still good to get an idea of all of the functions / features so here we go.

vCenter can be a bit of a memory hog, and given our limited resources I really don't want to force my home lab box into memory crunching.  With 32GB of RAM in the host, 2x ESXi virtual machines each with 4GB of RAM and 1x Domain Controller with 2GB of RAM I am using roughly 19% of my total physical memory available (according to Windows) - that is pretty efficent given that I have assigned about 32% of the total system memory to virtual machines alone, never mind Windows 8.1 running, my anti-virus, etc... etc...  However when I boot the VCSA which has 8GB assigned, my utilization jumps to almost 50% and I have a lot more virtual machines to deploy!  After finishing the VCSA setup wizard I shut the virtual machine off, edited the VM settings to only assign 4GB of RAM - <a href="http://www.virtuallyghetto.com/2013/08/quick-tip-minimum-amount-of-memory-to.html" target="_blank">thanks again to William Lam for the research on that</a>.  Now power back on the VCSA, in my environment I went from 50% down to 33% - a nice savings for sure.

So, time to log in and get vCenter setup, navigate to https://192.168.6.6:9443 assuming you are using the same IP scheme as me.
<blockquote>A quick aside, you may be wondering why I am using IP's instead of FQDN's - my host OS, where I am doing all my work from is not using my lab DC for DNS, thus I have no way to resolve the names without editing my host file.  If you want to access vCenter by name, simply add an entry to your host file or point your DNS to the IP of your domain controller.</blockquote>
When the vSphere Web Client login page comes up, log in as administrator@vsphere.local with the password you set during the VCSA configuration wizard.  Once logged in, the first thing you want to probably do is get rid of that pesky welcome screen, however if you are new to the vSphere Web Client take some time to check it out before disabling it and check out <a href="http://www.virtxpert.com/?s=web+client+tips">my other tips on the vSphere Web Client</a>.  So here we go...
<h3>vCenter Setup:  Permissions</h3>
<ul>
	<li>Once logged into the vSphere Web Client click on Administration &gt;&gt; Configuration</li>
	<li>If your domain is not listed, click on the green + icon</li>
	<li>Select the first radio button under Identity Source Type:  Active Directory (Integrated Windows Authentication)</li>
	<li>Ensure use machine account is selected and click the OK button</li>
</ul>
<img src="/images/fulls/sso-ad-added.png" class="fit image">
<ul>
	<li>Select your domain and click the the CD looking icon with the blue arrow, this will set your domain as the default so you can log in as adusername instead of adusername@domain.tld</li>
	<li>Click on Users and Groups, change the Domain pull down menu to your domain; ensure you can see users in your Active Directory and do not receive any error messages</li>
	<li>Click on the home link &gt;&gt; vCenter &gt;&gt; vCenter Servers; click on your vCenter server - in my case vxprt-vc01</li>
	<li>Click on the manage tab &gt;&gt; permissions</li>
	<li>Click on the green + icon, then click the Add button</li>
	<li>Ensure the domain pull down is your Active Directory Domain</li>
	<li>In my AD I have created a group called vcAdmins and an administrative user for myself; jfadmin which is a member of the vcAdmins group.  In the search box type vcAdmins or which ever group name you wish to assign full administrative privileges to.</li>
	<li>Highlight the group, click the Add button then click OK</li>
	<li>In the Assigned Role pane select Administrator</li>
</ul>
<img src="/images/fulls/vcenter-ad-group-admins.png" class="fit image">
<ul>
	<li>Click the OK button and ensure the group was added</li>
	<li>Log out of vCenter and log back in with a user account that is part of the vcAdmins group</li>
	<li>Once signed back in, click on vCenter and confirm you can see the vCenter server in inventory</li>
</ul>
<h3>vCenter Setup:  Create Datacenter</h3>
Now that we are logged into vCenter with an administrative user, it's time to setup our datacenter.  Now a data center is really just a construct/container in vCenter.  If I wanted I could create multiple data centers inside vCenter even if all of the compute resources were in the same physical location, even in the same rack, even sharing the same network.  What I would not be able to do as of vSphere 5.5 is vMotion or "live migrate" virtual machines from one data center to another.  In my lab I will be creating a single data center.
<ul>
	<li>Click on Hosts and Clusters</li>
	<li>Ensure you are on the summary tab and see vCenter in the Navigation pane</li>
	<li>Right click on your vCenter and select New Datacenter...</li>
	<li>As you can see from the wizard, there isn't a lot we can do in terms of settings at the data center level, name your datacenter, I prefer short names so I am going with dc01; click the OK button</li>
</ul>
A quick aside here; keep in mind when naming components in vCenter that down the road you may need to integrate other products.  For example in the past I have seen problems when trying to use Vagrant to bring up virtual machines in vSphere because of spaces in the names datacenters or clusters.  Keep your naming simple but identifiable.  I also prefer all lower case for my naming scheme.
<h3>vCenter Setup:  Create Cluster</h3>
With the datacenter created, we can now create our clusters.  Clusters play a big role in vSphere so while they are simple to create, managing the settings at the cluster level are vital to understand.  We'll get into that in a future post but just know a cluster is where your add resources such as compute/servers and configure VSAN, EVC, HA and DRS, assuming you have the appropriate licensing.
<ul>
	<li>Right click on your newly created datacenter and select new cluster</li>
	<li>You can see that there are quite a few options here for the cluster, we won't enable them all now but have a peak through and see what is available</li>
	<li>Name your new cluster, again I prefer simple so cl01 will be my cluster name; click the OK button</li>
</ul>
With different physical hosts, EVC is one item you may have enabled right away.  EVC "standardizes" on a processor feature set so that virtual machines can move between different physical hosts with different processors, assuming those processors can share a common processor feature set.  You cannot mix Intel and AMD processors.  In this home lab build with all virtual ESXi hosts, our hosts processors will all be identical.
<h3>vCenter Setup:  Add hosts to cluster</h3>
Now with our cluster build, we can now add the two virtual ESXi hosts we created in workstation earlier.  There will be warnings/errors after adding the hosts, don't worry those are expected and we'll take care of them!
<ul>
	<li>Right click on the cluster you just created and select Add hosts...</li>
	<li>Type the hostname of one the ESXi hosts you created in DNS, in my case vxprt-esxi01 and vxprt-esxi02, click next</li>
	<li>Type the username (root) and password you set during setup, click next</li>
	<li>When prompted to accept the certificate, click yes</li>
	<li>Review the information and click next</li>
	<li>On the license screen you can use the trial license or if you have license keys, enter the key here and click next</li>
	<li>Click next on the lockdown mode screen without selecting the Enable lockdown mode checkbox (lockdown mode disables access to the DCUI)</li>
	<li>Click finish, the host will be added to the cluster</li>
	<li>Repeat for the remaining virtual ESXi hosts you created</li>
</ul>
If you added local storage to your first ESXi hosts like we did in a previous posts you should see that host added with no warnings.  The 2nd ESXi hosts has no storage available for logs because it was only setup with a 1GB drive for the OS.
<h3>vCenter Setup:  Set NTP for ESXi hosts</h3>
NTP is critical for your envornment to work properly, if not set you will starting running into issues - especially when we get to vCAC/vRA.
<ul>
	<li>From the vSphere Web Client click vCenter &gt;&gt; Hosts and Cluster; expand the datacenter and cluster then click on one of your ESXi hosts</li>
	<li>Click the Manage tab &gt;&gt; Settings &gt;&gt; Time Configuration</li>
	<li>Click the Edit button, chose the Use Network Time Protocol radio button</li>
	<li>In the NTP servers field enter the FQDn or IP address of your domain controller (or dedicated NTP server or host record)</li>
	<li>Change the Startup Policy pull down to Start and Stop with host (stop is a little redundant here, pretty sure NTP won't run if I shut the host down :) )</li>
	<li>Click the Start button, then the OK button.  Repeat for your other hosts.</li>
</ul>
I appear to have to run into this little gem again; my ESXi hosts won't sync with my domain controller.  I know this works because I do it in my EMC labs all the time yet here I am looking at my old posts again.  <a href="http://www.virtxpert.com/vmware-esxi-5-1-will-not-sync-time-with-windows-2008-r2-ntp-domain-controller/">If your ESXi host won't sync time have a read</a>.

You now have the basics of a working vCenter setup, up next is some of the more advanced features like vMotion and HA!

<img src="/images/fulls/vcenter-built.png" class="fit image">