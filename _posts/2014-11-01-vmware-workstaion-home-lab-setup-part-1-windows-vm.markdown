---
layout: post
author: Jonathan Frappier
title: VMware Workstation Home Lab Setup Part 1 - Windows VM
permalink: /:title/
modified: 2014-11-01
category: articles
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---

In what should be a multi-part series (unless work gets insane) I will be setting up the supporting infrastructure for my home lab.  For this lab I will be using the <a title="8-Core, 32GB RAM, 360GB Flash, 3TB, Dual-NIC Home Lab Part List" href="http://www.virtxpert.com/8-core-32gb-ram-360gb-flash-2tb-dual-nic-home-lab-part-list/">8-core home lab</a> build I wrote about in the past.  I am currently running Windows 8.1 with VMware Workstation 10.  I have two volumes setup in Windows that will be dedicated for VMs - 1 is a single 120GB Neutron SSD that I will use for some of the "heavier" VMs such as SQL server and the vRealize IaaS server.  The other is a ~1.3TB RAID0 dynamic volume built in Windows on 3x 500GB Seagate hybrid drives which will be used for common VMs such as the domain controller I am setting up here.

I will be starting with all the VMs using NAT in VMware Workstation.  First I am setting up a Windows VM that we will use throughout the lab build - why am I doing this first, mostly because of how long patches are going to take to be totally honest, you could just as easily start with your virtual ESXi boxes (should be the next post) but alas here you are reading this.

First, create a new virtual machine in VMware Workstation

<img src="/images/fulls/vmware-workstation.png" class="fit image">

<ul>
	<li>On the New Virtual Machine wizard page select Custom (I prefer control over which settings I chose) and click Next
Select Workstation 10.0 and click Next</li>
	<li>Select I will install the operating system later radio button (old habit I'm hanging onto from old Workstation and Ubuntu days) and click Next</li>
	<li>Select Microsoft Windows and select the version from the pull down menu. I am using Windows Server 2012; click Next.</li>
	<li>If you have set your drives up like me, click the browse button and select your preferred Windows volume, in my case I have selected the "V" drive where my RAID0 volume is. I also have create a folder on this drive called VMs because OCD.</li>
	<li>Name your virtual machine and pasted that along with a leading \ into the location field aver V:\VMs to create the VM in its own folder like so and click the Next button.  In my setup I am actually using vxprt-win-tmp01:</li>
</ul>

<img src="/images/fulls/vmware-workstation-destination.png" class="fit image">

<ul>
	<li>I am staying with a single processor, single core - after all we don't have unlimited resources in this home lab, click the Next button</li>
	<li>I'm also sticking to 2GB of RAM (Next), NAT (Next), LSI Logic SAS (Next), SCSI (Next), and creating a new virtual disk (Next)</li>
	<li>On the Specify Disk Capacity page, I typically chose to store my virtual disks as a single file, this is up to you - I don't like having a bunch of files in my VM folder, feels messy. Also leave Allocate all disk space now unchecked to thin provision your disk and click Next</li>
	<li>Optionally you can rename your disk file, this again I prefer to have the same as my VM name, click Next and Finish. Your VM will be created, albeit with no OS yet.</li>
	<li>Right click on your new VM and select settings</li>
	<li>Click on CD/DVD and select the appropriate option to install Windows, in my case I have a downloaded ISO so I have selecte the Use ISO image file radio button and selected the desired ISO image.  Click OK to close the settings window.</li>
	<li>Right click on your VM, go to Power and click on Start Up Guest.</li>
</ul>
From here on out you've got a standard Windows install wizard to follow.  Once Windows is installed and you set your password, install VMware Tools by right clicking on the VM and selecting Install VMware Tools - follow that wizard, reboot and patch your Windows VM.  Next up a quick post on cloning VMs in Workstation so we can get to the fun part.