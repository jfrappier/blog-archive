---
layout: post
author: Jonathan Frappier
title: Configure Windows 2012 Failover Cluster for SQL 2014
permalink: /:title/
modified: 2015-03-16
category: articles
tags: [vmware, vsphere, vcenter, sql, failover cluster, sql]
share: true
---
Working on building out a lab that is going to be used to demonstrate setting up a vCenter environment. We were fortuneate enough to be given some time to set it up "right" - meaning setup a SQL cluster for vCenter, SSO in HA behind a load balancer with valid certificates. I drew the SQL straw, and it s the first time I have setup SQL clustering. I had to pull from a few different resources, none were completely what I was trying to do but thank you to Derek Seaman's blog and the MSDN blogs for being able to answer questions when they came up. You can find more information on <a href="http://pubs.vmware.com/vsphere-55/topic/com.vmware.ICbase/PDF/vsphere-esxi-vcenter-server-55-setup-mscs.pdf" target="_blank">Windows Failover Clustering on vSphere 5.5 </a>here (nope not on 6 yet). An over view of our setup:
<ul>
	<li>Two Windows 2012 R2 virtual machines on separate hosts; SQL1 and SQL2</li>
	<li>Each virtual machines with two NICs; one for production/client access the other for cluster communication.</li>
	<li>Each virtual machine has 2 drives; 60GB "C" for OS and 20GB "D" for SQL installation</li>
	<li>3 XtremIO drives presented via VPLEX</li>
	<li>AD accounts for SQL and SQL Agent were created in AD</li>
	<li>IP addresses for each of the SQL virtual machines, the Windows cluster, and the SQL cluster; for this setup that is 4 total.</li>
</ul>
Windows was installed, patched and joined to the domain. On each virtual machine I ensured that Windows Ethernet0 was first in the biding order and used for "production." NIC1 would be used for cluster communication. Ensure <a href="http://blogs.msdn.com/b/saponsqlserver/archive/2012/01/12/network-settings-network-teaming-receive-side-scaling-rss-amp-unbalanced-cpu-load.aspx" target="_blank">RSS is not enabled</a> on the NICs.

<ul>
	<li>In Windows 2012 R2 open the Network Connections window, click on Organize &gt;&gt; Layout &gt;&gt; Menu Bar.</li>
	<li>Once the menu bar appears click on Advanced &gt;&gt; Advanced settings. Ensure Ethernet0 is listed first.</li>
</ul>
<img src="/images/fulls/ethernet-order.png" class="fit image">

With networking setup, add the .NET 3.5 (needed for SQL), and Failover Clustering features. Next, shutdown SQL1, and SQL2. On SQL1 I added 3 new drives as physical RDMs; 1 each for the vCenter, and vRealize Automation database files and 1 for a quorum volume. The mapping file will need to be stored on a shared datastore that is accessible by both hosts. When adding these to SQL1, add 1 at a time, select the next available SCSI adapter - for example the first volume was set to 1:0, the 2nd 2:0, and the quorum volume 3:0. The OS (C), and SQL (D), drives were 0:0 and 0:1 respectively. A few considerations:
<ul>
	<li>In this format you will use all 4 of the possible SCSI controllers</li>
	<li>The SCSI controllers for the RDMs must have their SCSI Bus Sharing set to Physical (the virtual machine has to be off to do this, thus why I mentioned shutting down SQL1)</li>
	<li>Drives need to be added in the same order on both SQL1 and SQL2</li>
</ul>
Repeat adding drives to SQL2, except this time select "Use an existing virtual disk" and navigate to the SQL1 folder on the datastore to select the mapping file. You can check SQL1 to ensure that the correct mapping file is on the correct SCSI controller. For example, here you can see that SQL1_2.vmdk is on SCSI 1:0, so I want to make sure that SQL1_2.vmdk is using 1:0 on SQL2.

<img src="/images/fulls/sql-rdm.png" class="fit image">

Power on SQL1, and SQL2; configure each of the drives in Server Manager &gt;&gt; File and Storage Services on SQL1. <strong>DO NOT</strong> configure the drives on SQL2, when you build the cluster, and ultimately fail the cluster over from SQL1 to SQL2 it will mount those volumes. SQL2 should only have C (OS) and D (SQL) configured.

<img src="/images/fulls/sql1-drives.png" class="fit image">

Now that the drives on SQL1 are configured, open Server Manager &gt;&gt; Tools &gt;&gt; Failover Cluster Manager and click Create Cluster... Follow the wizard to add both SQL1 and SQL2. You will likely receive warnings about drives 0 and 1 since they can't be used for the cluster. When prompted, <strong>DO NOT</strong> select the option to add available storage, otherwise it will add D. We only want to add the RDMs to the cluster. During this process is you will also create an AD object for the Windows cluster and assign 1 of the 4 IP addresses mentioned earlier (2 others were already given to SQL1 and SQL2)

Once the cluster is created, expand Storage and click on Disks. Uncheck the 20GB D drive, in my case I added 2x 250GB volumes, and the 50GB volume. The 50GB volume will be the quorum drive. Once added they should all be online. Click on your SQL cluster, and in the Actions pane click More Actions &gt;&gt; Configure Cluster Quorum Settings...

<img src="/images/fulls/configure-quorum.png" class="fit image">

When the Wizard opens, click Next and select Use default quorum configuration; in my case it correctly selected the 50GB volume for the quorum witness. Now if I go back to Storage &gt;&gt; Disks you can see Disk 3 is set as the Disk Witness in Quorum.

<img src="/images/fulls/sql-drives.png" class="fit image">

Next create an AD object for the SQL cluster, I opted to go with <a href="http://blogs.msdn.com/b/psssql/archive/2013/09/30/error-during-installation-of-an-sql-server-failover-cluster-instance.aspx" target="_blank">option #2 for from this MSDN post</a>. This allows the AD Windows cluster object to manage the SQL AD object. You can finally start the SQL installer. When the SQL Installation menu opens, click Installation &gt;&gt; New SQL Server Failover Cluster Installation.

Select the options you wish for SQL, ensure that you install to the appropriate drive - in the case of this example D. Verify the installer correctly identified the RDMs as the data destination, and complete the installation. Once the installation finishes, enable FILESTREAM on the SQL Server Service. Start SQL Server Configuration Manager, click on SQL Server Services, right click on SQL Server and select properties. Click on the FILESTREAM tab and enable for both SQL and I/O access.

<img src="/images/fulls/sql-server-filestream.png" class="fit image">

Start the SQL installation on SQL2, except this time select Add Node to a SQL Failover Cluster. Select the cluster and most of the configuration options will be pulled from SQL1 (including the SQL installation directory, in my case D). Once complete you should be able to connect to the SQL instance, I did a named instance and called it VC so I connect to sqlc1\vc

<img src="/images/fulls/sql-cluster-management-studio.png" class="fit image">

Apologies for lack for explicit steps, it would have been quite a lengthy post but happy as always to answer any questions I can. Here is a quick demo where I connected to the instance, created a DB, ran a query to list all the DBs, powered off SQL1 (hard shutdown, no role draining) and re-ran the query. It took a few seconds for the failover to happen but was fairly quick.

<h3>SQL Failover Cluster</h3>

<iframe src="https://www.youtube.com/embed/MaZsrUaOzgc?rel=0" width="640" height="360" frameborder="0" allowfullscreen="allowfullscreen"></iframe>

<h3>SQL Failover Cluster Hosting vCenter Database</h3>
<iframe width="640" height="360" src="https://www.youtube.com/embed/780NVSlf5LI?rel=0" frameborder="0" allowfullscreen></iframe>