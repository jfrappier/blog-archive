---
layout: post
author: Jonathan Frappier
title: Install ESXi on a UCS boot from SAN LUN
permalink: /:title/
modified: 2014-03-18
category: articles
tags: [ucs, boot from san, esxi, lun, install]
share: true
---

This post will walk through mounting an ESXi ISO via the UCS KVM and installing ESXi.  It assumes that UCS Manager is configured properly with the necessary profiles, policies and pools, that all VLANs and uplinks have been properly configured and that storage switches and arrays have been properly zoned.  You should also verify the hardware and firmware version to download the correct drivers and build a custom ESXi ISO using PowerCLI if one does not already exist.
<ul>
	<li>Launch the Cisco UCS Manager; if it was not already installed/launched navigate to the IP/URL of your UCS manager (ex https://10.10.10.10) and click the Launch UCS Manager button, a jnlp file will be downloaded, launch that file which will load the UCS Manager and login window.</li>
</ul>
<img src="/images/fulls/starting-ucs-manager.jpg" class="fit image">
<ul>
	<li>Once logged in, you will see your equipment tab.  Expand <strong>Chassis</strong>, <strong>Chassis 1</strong> and then <strong>Servers</strong>.</li>
	<li>Click on the server you want to install ESXi on and click on the <strong>KVM Console</strong> action item.  A separate java application will launch.  If prompted about an unencryted KVM session, select Accept for this session, and remember, then Apply.</li>
</ul>
<img src="/images/fulls/ucs-action-menu.jpg" class="fit image">
<ul>
	<li>Click on the Virtual Media tab (accept again if prompted), click on Add image... and browse to your ISO directory, select the ISO and click the Mapped checkbox</li>
	<li>Power on the server, or if already booted click Reset (Click OK, Power Cycle, OK, OK)</li>
	<li>The UCS blades take several minutes to do checks, do not worry at this point.</li>
</ul>
<img src="/images/fulls/ucs-bios-test.jpg" class="fit image">
<ul>
	<li>When the Cisco splash screen appears, press F6</li>
	<li>When the boot menu appears, select EFI:  Cisco vKVM-Mapped vDVD and hit enter.  The KVM needs to read the ISO, depending on the connection between your ISO directory and KVM, this will take a few moments.</li>
</ul>
<img src="/images/fulls/ucs-select-dvd2.jpg" class="fit image">
<ul>
	<li>You will now see the ESXi installer start to load.  From here, proceed with a normal ESXi installation.</li>
</ul>
<img src="/images/fulls/ucs-esxi-installer.jpg" class="fit image">

**Please note** It is commonly considered best practice to only zone the boot from SAN LUN during installation to prevent accidental installation and erase of other LUNs that are being used as datastores.

ESXi Installation Steps:  Enter, F11, select the LUN and hit enter, if a previous version of ESXi is/was installed you will be prompted for an install or upgrade and hit enter, select your keyboard and hit enter, enter a password and hit enter.  After several moments, you will be ready to install; press F11.

When the installation finishes, it will un-map the ISO, press Enter to reboot.  The following steps assume you need to assign a static IP address to your management interface.
<ul>
	<li>Once ESXi boots, press F2 and enter the password you set during installation.</li>
	<li>Use the arrow keys to select the Configure Management Network option and hit enter</li>
	<li>Select Network Adapters and select the interface you wish to use (depending on your UCS configuration, NIC0 may be adequate)</li>
	<li>Select VLAN (optional) and enter the VLAN of your management network</li>
	<li>Select IP Configuration and enter the IP address, subnet mask and gateway</li>
	<li>Select DNS configuration and enter the DNS servers and FQDN of your host</li>
</ul>
You should now be able to connect with the C# client or to vCenter