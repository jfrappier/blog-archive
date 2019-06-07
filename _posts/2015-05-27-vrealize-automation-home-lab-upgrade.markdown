---
layout: post
author: Jonathan Frappier
title: vRealize Automation Home Lab Upgrade
permalink: /:title/
modified: 2015-05-27
category: articles
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
---
With new versions of vRealize Automation and vSphere dropping, and seemingly being stable it is time to upgrade the home lab. Since this is a home lab, and somewhat basic there are just a few steps from <a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=2109760" target="_blank">KB2109760 </a>that needs to be followed:
<ol>
	<li>Upgrade vRA (Appliance &gt;&gt; IaaS)</li>
	<li>Upgrade Application Services</li>
	<li>Upgrade vCenter</li>
	<li>Upgrade ESXi</li>
</ol>
In this post, I will cover the first step in the process, upgrade vRealize Automation to 6.2.latest. First, I have shut down services on my IaaS server. Now log into the VMware vCAC Appliance management interface on port 5480 - in my case https://vxprt-vcac01.vxprt.local:5480 for example and click on the update tab. Now, click on Check Updates. As you can see here, I have an available updated from 6.1.1.0 to 6.2.1.0

<img src="/images/fulls/vcac-upgrade.png" class="fit image">

Now, as you might expect, click on Install Updates &gt;&gt; OK. The upgrade process will begin.

<img src="/images/fulls/vcac-upgrade-starting.png" class="fit image">

After a few minutes, you should be presented with a message that a reboot is required.

<img src="/images/fulls/vcac-upgrade-complete.png" class="fit image">

Click on the System tab, click the Reboot button, and click the Reboot button again; the system will reboot. Once the reboot completes, you should be able to log in and verify the version by clicking on the system tab. Notice anything different?

<img src="/images/fulls/vra-branding.png" class="fit image">

The updated product name; vRealize Automation is now displayed instead of vCAC Appliance and the version is 6.2.1.0. Once all the services have started, you should also be able to log into the vRealize Automation console and see the tenant information from the previous configuration.

<img src="/images/fulls/vra-console.png" class="fit image">

The next step is to upgrade the IaaS components. Again this should be straight forward in a lab because all of the components are on a single server, and not distributed. Log into the IaaS server as the service account used to run the IaaS components, if you followed along in my vDM 30-in-30 challenge you would have named it something along the lines of svc_vra_iaas. Open a web browser and grab the vRA 6.2 PreReq script Brian Graf has built over on GitHub (<a href="https://raw.githubusercontent.com/vtagion/Scripts/master/vCAC62-PreReq-Automation.ps1">https://github.com/vtagion/Scripts/blob/master/vRA%206.2%20PreReq%20Automation%20Script.ps1</a>). Save, open a PowerShell console as administrator and run the script.

<img src="/images/fulls/vra-iaas-script-upgrade.png" class="fit image">

Follow the prompts in the prereq script, typically I have selected option 2 - I have internet access and want to download and install it automatically.

<img src="/images/fulls/vra-iaas-net-upgrading.png" class="fit image">

Select option 2, 2 more times. When prompted, provide the service account for the IaaS components, in my case vxprt\svc_vra_iaas and the script should complete.

<img src="/images/fulls/vra-script-complete.png" class="fit image">

Now, navigate to the vRA appliance page. Click on the vRealize Automation IaaS installation page link, download and extract the zip file containing the database upgrade scripts. From a command prompt run the following command:

dbupgrade.exe -S {servername\instancename} -d {dbname} -E

On my server I am using the default SQL Express instance, so the instance name is not needed, and my DB name is vCAC so my command looks like this:

dbupgrade -S localhost -d vCAC -E

<img src="/images/fulls/db-upgrade.png" class="fit image">

If you are receiving any errors, make sure that Named Pipes is enabled.

<img src="/images/fulls/sql-named-pipes.png" class="fit image">

Now that the DB is upgraded, download the IaaS Installer file, do not rename the file, and run it. The upgrade is of the next, next, next variety.
<ol>
	<li>Click Next</li>
	<li>Accept the terms and click next</li>
	<li>Enter the root password for the vRA appliance, accept the certificate, and click Next</li>
	<li>Upgrade should be the only option that is available, click Next</li>
	<li>Enter the service account password, the server name, database name, and click Next</li>
	<li>Click the Upgrade button</li>
</ol>
If the computer gods are on your side, the installation should complete

<img src="/images/fulls/iaas-upgrade-done.png" class="fit image">

Click Next and Finish. If you flip back over to your vRA console, you should see all of the available tabs based on the user permission - in this case my iaasadmin user.

<img src="/images/fulls/vra-portal.png" class="fit image">

Up next is upgrading Application Services.