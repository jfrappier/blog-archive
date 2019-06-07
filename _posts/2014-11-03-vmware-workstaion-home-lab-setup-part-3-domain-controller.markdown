---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 3 - Domain Controller
permalink: /:title/
modified: 2014-11-03
category: articles
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---
With our Windows virtual machine built, patched, and cloned, its time to setup the Domain Controller for the home lab.  We will use the Domain Controller for authentication throughout the home lab setup including the necessary service accounts for VMware vSphere, SSO and vCloud Automation Center/vRealize Automation.  If you are here from part 2 you should be looking at a booted virtual machine clone here are the steps to finish off the Windows system wizard - if you already blew through this no worries you can skip the next section.
<ul>
	<li>Tick the I accept box and click the Accept button.</li>
	<li>Set your region, language and keyboard layout and click Next</li>
	<li>Set your administrator password and click Finish</li>
	<li>You should now be at the login screen.</li>
</ul>
Now its time to setup this Windows VM as our Domain Controller - it used to be quite easy - type
<pre>dcpromo</pre>
and follow the wizard, unfortunately Microsoft in all their wisdom decided to change the process after 12 years of it working flawlessly.
<ul>
	<li>Press CTRL-ALT-INS on your keyboard or click on the VM menu and select Send CTRL-ALT-DEL (CTRL-ALT-INS seems much easier to me)</li>
	<li>Log in with the password you just set</li>
	<li>First update the date/time in Windows so it is in the correct time zone. You can click on the clock in the lower right corner or bring up the Date and Time control panel item</li>
	<li>Once the date is set, click on the Internet Time tab, ensure it is set to automatically synchronize with time.windows.com and click OK</li>
	<li>In this setup, I will use the domain controller as an NTP server so I can point my ESXi virtual machines and other appliances here so time is synchronized properly (NTP is critical in any environment, even the lab). In order to use Windows as an NTP server there is a registry change we need to validate. Bring up the start menu and type regedit</li>
	<li>Navigate to HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\W32Time\TimeProviders\Ntp - ensure that Enabled is set to 1 (this was already set to 1 for me)</li>
	<li>If you are not already there, open the Start menu, right click on Computer and select properties</li>
	<li>Under Computer name, domain and work group settings click the Change settings button the click the Change button</li>
	<li>Name your computer, I prefer short and simple so dc01 is pretty common for me but I'm going with vxprt-dc01 here.  Leave the Workgroup selected and click the OK button</li>
	<li>When the Computer Name/Domain change popup opens click OK, click Close on the System Properties window and then click the Restart Now button</li>
	<li>Once the virtual machine restarts, log back in as administrator</li>
	<li>Now, open Server Manager (it may already be open - I didn't say simon says close the Server Manager window :)</li>
	<li>Click on Add roles and features</li>
</ul>

<img src="/images/fulls/server-manager.png" class="fit image">

<ul>
	<li>When the Add Roles and Features Wizard starts, click Next</li>
	<li>Select Role-based or feature-based installation, click Next</li>
	<li>Ensure select a server from the server pool radio button is selected and that your server is highlighted, then click Next</li>
	<li>Check the box for Active Directory Domain Services, click the Ad Features button when prompted, and click Next</li>
	<li>Click Next on the features page</li>
	<li>On the AD DS page, click Next</li>
	<li>On the Confirm installation selections tick the box to Restart the destination server automatically if required (then Yes - it shouldn't need a reboot but hey why not) and then click the Install button.</li>
	<li>When finished, click the Close button</li>
</ul>
At this point, the necessary components have been put in place to configure Active Directory but nothing has been configured yet.  That's next!