---
layout: post
author: Jonathan Frappier
title: Install vRealize Automation IaaS Preparation - vRealize Automation Series Part 4
permalink: /:title/
modified: 2014-11-16
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---
Now that we have the vCloud Automation Center / vRealize Automation appliance deployed and working, it is time to get the Infrastructure-as-a-Service components installed.  The IaaS components run on Windows and need to connect a Microsoft SQL server instance.  In my case I am going to use SQL express installed on the same VM I will install the IaaS components on, again we are in a lab here - typically you would have a separate database server to host the DB.  As always, and a point I've not referenced really at all this month, always check the documentation to ensure you are installing on a supported platform.  The <a href="https://www.vmware.com/support/pubs/vcac-pubs.html" target="_blank">vCloud Automation Center / vRealize Automation documentation</a> is excellent, as is the <a href="http://www.vmware.com/pdf/vcloud-automation-center-61-support-matrix.pdf" target="_blank">support matrix PDF</a>.

Since Microsoft SQL Server 2014 is supported, that is what I will use.  Also, as of 6.1 you can now install the IaaS components on Windows Server 2012 R2, in 6.0 you could not use R2 because of the native .NET version that shipped with R2 (4.5.1).  I am using Windows Server 2012 here because that happens to be what I have downloaded but the same steps should work fine for R2.  A few prerequisite steps to take care of first:
<ul>
	<li>Log into your domain controller and create a user account for the IaaS components to run as, something like svc_vra_iaas; we will give this account administrative permission to the IaaS virtual machine and in SQL to create the database</li>
	<li>Create a group called iaasAdmins; we will put the user account in this group and give the group permission on the server</li>
</ul>
First step is to clone another Windows 2012 virtual machine in VMware Workstation, <a href="http://www.virtxpert.com/vmware-workstaion-home-lab-setup-part-2-attack-of-the-clones/">see Part 2 of my home lab series</a> if you need a walk though on setting up the template or on how to do the clone.  Once you have your clone, boot your virtual machine, name it accordingly and join it to your domain.
<ul>
	<li>Log in as an administrative user, we will grant access to our administrative user we created in AD to perform the rest of the tasks</li>
	<li>In Server manager click on Tools &gt;&gt; Computer Management &gt;&gt; Local Users and Groups &gt;&gt; Groups</li>
	<li>Right click Administrators &gt;&gt; Add to Group...</li>
	<li>Click the add button and enter iaasAdmins, log in with domain credentials (if you logged in as the local admin)</li>
	<li>Click OK twice and Close Computer Management</li>
	<li>You should now be back in the Server Manager Dashboard (if not open Server Manager) and click on Local Server</li>
	<li>Disable Windows Firewall and IE Enhanced Security Configuration</li>
	<li>Click back on Dashboard, then Add roles and features</li>
</ul>
<img src="/images/fulls/windows-server-2012-add-roles-features.png" class="fit image">
<ul>
	<li>When the wizard starts, click Next</li>
	<li>Select Role-based or feature-based and click Next</li>
	<li>Ensure your server is selected and click Next</li>
	<li>On the roles page just click Next</li>
	<li>On the Select Features page expand .NET Framework 4.5 Features and select all options under WCF Services, when prompted add any required features</li>
</ul>
<img src="/images/fulls/windows-server-2012-wcf-services.png" class="fit image">
<ul>
	<li>Once all have been selected, click Next, then next 2 more times</li>
	<li>On the confirm installation selections page click the box to restart the destination server automatically if required (it shouldn't) and click the Install button; the installation will start</li>
	<li>Once finished click the close button</li>
</ul>
Okay so a little of that may have been left over from the 6.0 days, it looks like the new pre-req script from Brian Graf may handle most of that, in any case know that we could have had a lot of boxes to tick here but we can avoid that error thanks to Brian's script.  You can <a href="http://blogs.vmware.com/PowerCLI/2014/09/vcac-6-1-pre-req-automation-script-released.html" target="_blank">download the script from http://blogs.vmware.com/PowerCLI/2014/09/vcac-6-1-pre-req-automation-script-released.html</a>.  Save it to a folder on the IaaS server, I prefer c:\admin\scripts but you can put it where ever you like.
<ul>
	<li>Once the script has been downloaded and extracted, open PowerShell as an administrator (right click Run as Administrator) and run</li>
</ul>
<pre>Set-ExecutionPolicy Unrestricted</pre>
In production you shouldn't do that, RemoteSigned maybe then unblock the script - follow your security policies there.
<ul>
	<li>Log out of your IaaS server and log back in as the user account we created, in my case svc_vra_iaas</li>
	<li>Change to the directory where you downloaded and extracted the script</li>
	<li>Run vCAC61-PreReq-Automation.ps1</li>
	<li>Answer any questions the script presents specific to your environment</li>
	<li>Some of the steps could take a long time, so be patient while it runs (e.g. downloading and installing .NET updates)</li>
</ul>
<img src="/images/fulls/vcac-61-prereq-automation-script.png" class="fit image">

This is a very good script, if you have any errors (text in red) check them and re-run the script, otherwise we are ready to install Microsoft SQL in our next post!