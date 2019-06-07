---
layout: post
author: Jonathan Frappier
title: Back to basics - Installing VMware ESXi 6
permalink: /:title/
modified: 2015-02-03
category: articles
tags: [esxi , vmware, vsphere, esxi installation, vsphere 6]
share: true
---
<em>**Please note that the installation steps here and requirements are based on beta and release versions of ESXi 6.**</em>

Installing VMware ESXi 6 is just as straight forward as ever, of course you’ll want to make sure your hardware is on the VMware HCL and you meet the necessary system requirements:
<ul>
	<li>64-bit x86 processor with VT-x or AMD-V enabled in the BIOS</li>
	<li>NX/XD enabled in the BIOS</li>
	<li>Dual-core/dual processor</li>
	<li>4GB RAM</li>
	<li>1GB of local storage</li>
</ul>
Of course those are minimums and you won't get much virtualized with those specs, but alas that is likely fine for lab and testing purposes.  For the installation, I typically suggest USB or SD card.  This saves your physical disks, either locally or in a boot from SAN configuration free for VM related IO.  If you have local disks and flash based drives in your system, you can enable VSAN for example to provide shared storage in from the local storage in your hosts. There are other requirements for VSAN that I’ll touch on in another post (or check out yellow-bricks or cormachogan.com/)

The local storage is the bare minimum required. With only 1GB there are a few extra steps after the installation to define a location for log storage but its a simple step. If you want storage for log files as part of your boot media, you will need at least 5.2GB. When you reach the root password step, usually I start with something easy to type so when I log into the console interface (DCUI) after the installation and add the hosts to vCenter I’m not “infomercial bumbling” for the password. Later I can then rip a <a title="PowerCLI – Update ESXi Root Password with Password Generator" href="http://www.virtxpert.com/powercli-update-esxi-root-password-password-generator/">PowerCLI script through the environment to change to a more complex password</a>.

Burn the ISO do a CD or mount it in your remote console (e.g UCS, iLO, DRAC or vSphere/Workstation/Fusion for your nested home lab) and power on the computer.

The ISO will launch into the installer:
<ul>
	<li>Press Enter to start the installation wizard</li>
</ul>

<img src="/images/fulls/1-esxi-6-install-welcome.png" class="fit image">
<ul>
	<li>Press F11 to accept the EULA</li>
	<li>Select the disk you wish to install ESXi on to and press enter</li>
</ul>
<img src="/images/fulls/2-esxi-6-install-select-disk.png" class="fit image">
<ul>
	<li>Select the keyboard layout and press enter</li>
	<li>Enter your root password and press enter (at this point I tend to use something easy to type, just make sure to change later if you follow this)</li>
</ul>
<img src="/images/fulls/3-esxi-6-install-password.png" class="fit image">
<ul>
	<li>Press F11 to start the installation</li>
</ul>
<img src="/images/fulls/4-esxi-6-install-f11-install.png" class="fit image">
<ul>
	<li>Remove the installation media and press enter to reboot</li>
</ul>
Once you have restarted, you will be at the Direct Console User Interface, aka the DCUI. That is it, installing ESXi, assuming you have the prereqs in place is quite straight forward, configuration on the other hand - well that depends on your environment and your business requirements. <a title="VMware Workstation Home Lab Setup Part 5 – ESXi Template" href="http://www.virtxpert.com/vmware-workstaion-home-lab-setup-part-5-esxi-template/">If you are installing ESXi in your lab as a nested virtual machine you may also want to consider VMware Tools for ESXi</a>.