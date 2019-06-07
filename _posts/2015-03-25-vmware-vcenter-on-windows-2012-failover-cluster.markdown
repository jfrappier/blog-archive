---
layout: post
author: Jonathan Frappier
title: VMware vCenter on Windows 2012 Failover Cluster
permalink: /:title/
modified: 2015-03-25
category: articles
tags: [vmware, vsphere, vcenter, cluster]
share: true
---
Some time around the release of vSphere 5.5 (Update 2 maybe?) VMware <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=1024051" target="_blank">officially(?) didn't not support vCenter on a Windows Failover Cluster</a>. I say didn't not support because there still seems to be very limited documentation and KB's on how to do this. The <a href="http://www.vmware.com/files/pdf/techpaper/vmware-vcenter-server-availability-guide.pdf" target="_blank">VMware vCenter Server Availability Guide</a> documents available options such as using HA for vCenter availability, but also how to install vCenter on a Windows Failover Cluster, and configure the services appropriately since the application itself is not other cluster aware, for example like <a title="Configure Windows 2012 Failover Cluster for SQL 2014" href="http://www.virtxpert.com/configure-windows-2012-failover-cluster-for-sql-2014/" target="_blank">installing SQL on a failover cluster</a>.

If you have done a failover cluster on Windows before, the process is a bit different so don't just dive in as I did. So what does my environment look like:
<ul>
	<li>SSO has been already deployed and working</li>
	<li>A management vCenter is running; you will need this or some other means to clone the first virtual machine after installation</li>
</ul>
So wait why are you clustering vCenter if there is already a vCenter you ask? Many reasons, but primarily our availability of our management vCenter is less of a concern. The clustered vCenter is being deployed to support vRealize Automation so end users will rely on this vCenter to be able to request catalog items. Availability was more of a concern for this purpose than strictly management.
<ul>
	<li>Start with only a single Window 2012 R2 64-bit virtual machine (not 2) as you will later clone this virtual machine to act as the 2nd node</li>
	<li>I placed the original, and clone on two separate physical hosts</li>
	<li>Each virtual machine has a single 60GB (C) drive for the OS</li>
	<li>2 additional volumes will be added which, in my case, are XtremIO volumes presented as a physical RDM. This should also work using in-guest iSCSI for example</li>
	<li>1 of the 2 additional volumes is a 60GB (D) drive which vCenter will be installed on and the other a quorum disk for the failover cluster</li>
	<li>Each virtual machine has two NICs - one for production/client access the other for cluster communication</li>
	<li>The Windows Failover Cluster will have an IP address, as well as the vCenter Service role which you will create; in total this is 6 IP address</li>
	<li>An AD account was created for the vCenter services, added to the local administrators group and given permission on the SQL server as required</li>
</ul>
A few notes before I review the process;
<ul>
	<li>If you are using RDMs, <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=1016106" target="_blank">make sure you read this KB to mark the RDMs as perennially claimed</a> otherwise storage rescans and boot times will be drastically affected (hosts were taking roughly an hour to boot)</li>
	<li>The directions have you install the vCenter Web Client, Inventory Service, and vCenter services to the D drive. There is a known bug that causes the web client to not function properly when installed to a non-default location (though it seems more that it doesn't work when not installed to the C drive). <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2044953" target="_blank">You'll need this KB article which walks you through creating a symbolic link</a>, after implementing this the web client operated as expected. Also, once installation is complete and working on the primary node, you'll need to failover to the secondary node to create the sym link (well at least I did, would it let you create a sym link to a drive that didn't exist? hmmm)</li>
</ul>
So, with that out of the way there is a few things to define before you bring up your first virtual machine - specifically the names and IP addresses of both virtual machines, the Window cluster, and the vCenter cluster.

<strong>This is important, and I misinterpreted this step the first time I did this: When you create the first virtual machine - give it the name and IP address of what will ultimately be the vCenter cluster - using the example above you will name the computer VC2, with an IP address of 192.168.1.100 and join it to your domain. After the initial install this will be changed.</strong>

Create the virtual machine, with 2 NICs and the RDMs. Mount one of the RDMs as D and one as whatever letter makes you happy, for my OCD that would be Q for quorum. Create your system DSN as you normally would, log in as your vCenter service account and perform a custom installation (not simple), installing each of the components to the D drive. During the installation process note that the name being added to SSO is the name that will ultimately be the vCenter cluster.

<strong>Before removing the RDMs, make sure to note their original file name, volume ID, and SCSI controller; they need to be added back in the same order.</strong>

These steps are pretty straight forward in the guide, change all of the vCenter services to manual, shutdown the virtual machine, remove the RDMs, and make a clone of the virtual machine. One item not clear was when to re-add the RDMs, I chose to play it safe and kept them out of the virtual machine for now. Once the clone is complete, power on the <strong>cloned</strong> virtual machine and rename it to the secondary vCenter node hostname and IP address. Power on the original virtual machine, unjoin it from the domain, rename and IP it with the hostname for the primary vCenter node, and rejoin the domain. Now you can power off the virtual machines, re-add the RDMs to the primary node, then the secondary as you typically would, making sure the SCSI controller is set to physical sharing.

Power on the virtual machines and install the Failover Cluster feature on each. Once complete, create a new cluster on the primary node - during the creation you will be asked for a cluster name and IP address - use the Windows Cluster name (VC2Win) from the example above - this is <strong>NOT</strong> the vCenter cluster name and IP address which you used on the initial virtual machine during installation. Unlike with the SQL post I wrote, you can add all available cluster storage as both additional drives are used for the cluster (D - App, Q - Quorum). Now that the cluster has been created, you should have an AD object called VC2Win. <a href="http://blogs.msdn.com/b/psssql/archive/2013/09/30/error-during-installation-of-an-sql-server-failover-cluster-instance.aspx" target="_blank">Using option #2 from this MSDN blog post</a>, create your vCenter cluster AD object. Failing to do this will cause the cluster to fail when you attempt to start it.

The rest of the steps for creating the vCenter cluster role are well documented with one caveat, so rather than copy paste them here finish reading the <a href="http://www.vmware.com/files/pdf/techpaper/vmware-vcenter-server-availability-guide.pdf" target="_blank">VMware vCenter Server Availability Guide</a>. That caveat, because your vCenter services were set to manual, and thus not started after the reboots, when you create the initial vCenter role service it will come us as <span style="color: #ff0000;"><strong>failed</strong></span> - which made me go  ZOMG not again! This message is actually just the status of the clustered service, which is stopped, thus failed from a Windows Failover cluster perspective - it is okay to proceed with creating the remaining services and setting the dependencies.

At this point, you should be able to start the cluster and have all services come up.

<img src="/images/fulls/vcenter2.png" class="fit image">

Once it is up, access the web client and set permissions as required. For example, as you can see in this screenshot, here is both vCenters in the web client after since my account was given the appropriate permissions to both.

<img src="/images/fulls/ohya.jpg" class="fit image">

The last item I have to tackle is automating the backup, copy, and restore of the ADAM database. There are a lot of words in the doc which basically says - xcopy the backup to the correct location. The document talks about stopping/starting services before placing the file. But if the services aren't running on VC2-2, I should just be able to drop it in. Now when the services start there is an up to date file which will get loaded.

So, quick a dirty like...

del d:\backup\*.* /Q
%windir%\system32\dsdbutil.exe "ac i VMwareVCMSDS" ifm "create full D:\backup" q q
xcopy /osy d:\backup\adamntds.dit "\\VC2-2\C$\ProgramData\VMware\Vmware VirtualCenter\VMwareVCMSDS"