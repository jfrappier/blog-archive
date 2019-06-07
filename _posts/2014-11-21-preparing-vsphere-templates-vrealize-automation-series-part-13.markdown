---
layout: post
author: Jonathan Frappier
title: Preparing vSphere Templates - vRealize Automation Series Part 13
permalink: /:title/
modified: 2014-11-21
category: vra
tags: [vmware, vsphere, vcenter, home lab, vra, vcac]
share: true
related: [
    "So you want to be a full stack engineer? Advice on mapping out your career", 
    "Adding Stand Alone Hyper-V – vRealize Automation Series Part 16", 
    "Deploy an Application Blueprint - Application Services Series Part 5"
]
---

In order to use vSphere templates in vRealize Automation / vCloud Automation Center and Application Services / Application Director there is a bit of preparation you need to do, especially if you want to use Application Services.  There are guest agents for both vRealize Automation and Application Services so lets get started.  A quick assumption here, you already have a linux virtual machine installed with VMware Tools.  I am going to cheat a bit here and use the e1000 NIC, if you want to use the VMXNET3 adapter <a href="http://www.virtxpert.com/help-installed-centos-vmxnet3-nic-cant-install-perl-vmware-tools/">see my post on how to install VMware Tools...which needs Perl...which needs network access...which needs Perl</a>!  Let's get started with the specifics on configuring your Linux VM; I have a CentOS virtual machine called vxprt-centos-tmp that is powered on and ready to configure.  Log in via the VMRC or SSH to get started:

Note that as of Application Services 6.1, you cannot use CentOS7 - at the very least the guest agent will not install, I have not tested beyond the agent installation so certain functionality may work.  The support matrix has more details on <a href="http://www.vmware.com/pdf/vcloud-automation-center-61-support-matrix.pdf" target="_blank">supported operating systems</a>.
<ul>
	<li>For linux, this is bundled into an installer</li>
	<li>Logged in as root run wget <span id="GUID-659B901A-0071-43E5-9FBE-D1BB4EA8725F__filepath_7510D5E961A048D19EBE93899A39B018" class="filepath">http://</span><var id="GUID-659B901A-0071-43E5-9FBE-D1BB4EA8725F__varname_CF15594695494C93B0DD35305C339C2A" class="varname">192.168.6.22/</var><span id="GUID-659B901A-0071-43E5-9FBE-D1BB4EA8725F__filepath_E6D6006D39854191BD79FCFD1EA9A013" class="filepath">tools/preparevCACTemplate.sh</span> - replace with your server name as necessary (I've not configured all network settings for this VM)
<ul>
	<li>If wget is not installed, run yum install wget</li>
</ul>
</li>
	<li>Type ls - notice preparevCACTemplate.sh is grey</li>
	<li>Now run chmod +x ./preparevCACTemplate.sh</li>
	<li>Type ls again, notice now its green; +x added execute permission on the script</li>
	<li>Now run the script;  ./preparevCACTemplate.sh - the vCloud Automation Center Agent Installer will start</li>
</ul>
<img src="/images/fulls/apps-agent-installation-linux.png" class="fit image">
<ul>
	<li>Enter the following information in the wizard:
<ul>
	<li>vCloud Automation Center Manager Service Server:  192.168.6.20</li>
	<li>vCloud Automation Center IaaS Server:  192.168.6.21</li>
	<li>Application Services Server:  192.168.6.22</li>
	<li>Check certificates:  n</li>
	<li>Download timeout:  Just press enter</li>
	<li>Download and install Java:  y</li>
	<li>When prompted click Y to install</li>
</ul>
</li>
</ul>
The installer will download all of the necessary components and place them in the correct location; a nice step forward from vCloud Automation Center and Application Director 6.0.   You should receive a message that the Installation Complete Successfully and Ready to capture as a template... however there is still one more step we actually need to do - remove the 70-persistent-net.rules file.  This file keeps track of MAC addresses and it will change every time we clone the template.  By removing it, it will recreate the file on first boot.
<ul>
	<li>Type cd /etc/udev/rules.d</li>
	<li>Type rm 70-persistent-net.rules</li>
	<li>Type y</li>
	<li>Type shutdown now -h to shutdown the virtual machine</li>
	<li>Return to the vSphere Web Client</li>
	<li>Right click on the powered off virtual machine and select All vCenter Actions &gt;&gt; Convert to Template</li>
</ul>
We should now be ready to add the vSphere template as a Blueprint in vRealize Automation Center.  Note you also need to understand what you will be deploying to your template, for example I ran into several problems because SELinux was enabled (and I wasn't aware that it was) so make sure to check everything from an OS and service perspective is in order before shutting down.