---
layout: post
author: Jonathan Frappier
title: Install vRealize Automation IaaS Installation - vRealize Automation Series Part 6
permalink: /:title/
modified: 2014-11-17
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---

With SQL now installed, it's time to download and install the IaaS or Infrastructure-as-a-Service components.  Log into your IaaS server as the service account you created and provided to the PreReq script - in my case svc_vra_iaas.  Once you are logged in, close Server Manager, open a web browser and navigate to the URL of your vCloud Automation Center / vRealize Automation appliance, for example in my case https://vxprt-vcac01.vxprt.local.  You should now be at the VMware vCloud Automation Center Appliance page where we can get started

<img src="/images/fulls/vcac-appliance-iaas-download.png" class="fit image">
<ul>
	<li>Click on the vCloud Automatin Center IaaS installation page link</li>
	<li>Click the link to download the IaaS installer - do not rename the installer</li>
	<li>Once downloaded, launch the installer by right clicking and select Run as Administrator - The vCloud Automation Center Configuration wizard will launch.</li>
	<li>On the Welcome page, click Next</li>
	<li>Accept the license terms and click Next</li>
	<li>Provide the username and password for the appliance, typically root, and click Next</li>
	<li>Here you could do either a Complete install which will put everyone on one server (our option) or a Custom install which allows you to install specific components on specific servers and build an HA configuration.  See the reference architecture document in the introduction post to learn more.  Click Next</li>
	<li>The PreReq checker will run.  Oddly enough in my case it says that Java is not okay, though when I ran the PreReq PowerShell script it said it downloaded and ran it fine.  If you run into any errors here cancel the install and re-run that script.  After re-running the PreReq script all tests passed, click Next</li>
</ul>
<img src="/images/fulls/vcac-iaas-installer-prereq-check-passed.png" class="fit image">
<ul>
	<li>Enter the password for your user account, a passprhase you wish to use for the database encryption key and verify the SQL server is the correct server.  If you were using a separate SQL server, here is where you would change that.  Since we configured SQL for Mixed Mode and provided the svc_vra_iaas user with sysadmin rights, we can leave Use Windows Authentication checked.  Once everything is filled in, click Next</li>
	<li>On the DEM and Agent page, make note of the values here, we will need them later when adding vCenter as an endpoint; click Next</li>
</ul>
<img src="/images/fulls/vcac-dem-agent-default-values.png" class="fit image">
<ul>
	<li>On the component registry page, click the Load button, vsphere.local should be filled into the field automatically</li>
	<li>Click on the Download button, once filled in click the Accept Certificate checkbox</li>
	<li>Fill in the SSO username and password, administrator@vsphere.local, then click the Test button</li>
	<li>Confirm the IaaS server FQDN was filled in properly, click the Test button</li>
	<li>With everything filled in and tested passed, click Next</li>
</ul>
<img src="/images/fulls/vcac-iaas-component-registration.png" class="fit image">
<ul>
	<li>On the Ready to Install page, click the Install button</li>
</ul>
The installation of the IaaS components, like some of the configuration on the appliance can take some time.  Watch the installation screen to see what it is doing - or get a cup of coffee :)  After a few minutes, you should get a message that the Installation completed - congratulations you have finished the easy part!
<ul>
	<li>Click the Next button</li>
	<li>On the Configuration is complete page, uncheck the Guide me through the initial system configuration box and click finish</li>
	<li>Return to you workstation or normal means of access the appliance web pages and log into your appliance (https://vxprt-vcac01.vxprt.local)</li>
	<li>Click on Tenants &gt;&gt; vsphere.local &gt;&gt; Administrators - notice anything different here?</li>
</ul>
Before installation of the IaaS components we couldn't assign any accounts to be infrastructure administrators because??? Yup - the Infrastructure-as-a-Server components were not installed!  With the vCloud Automation Center / vRealize Automation Appliance and IaaS components installed, we can now start to get everything configured.