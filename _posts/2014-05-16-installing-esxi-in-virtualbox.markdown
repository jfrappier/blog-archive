---
layout: post
author: Jonathan Frappier
title: Installing ESXi in VirtualBox
permalink: /:title/
modified: 2014-05-16
category: articles
tags: [vcsa, vmware, vsphere, vcenter server appliance, vsphere 6]
share: true
---
Why, well because you don't always have VMware Workstation or Fusion available, and VMware Player only let's you do so much.  Sometimes you just need to make due with VirtualBox.  Be warned, however, ESXi has been quite unstable for me in VirtualBox, especially if trying to add new hardware.  The build below was fairly stable until I tried to add a new virtual hard drive, now it PSODs on boot.  Plan ahead and add all necessary hardware prior to ESXi installation.

Here are the steps to setup your VirtualBox VM.

Don't forget to enable VT in your BIOS
- Click on the new button
- Give your VM some name, select Linux, Linux 2.6/3.x (64-bit)
***If you don't see 64-bit options, you didn't enable or don't have virtualization support in your processor***
- Give the VM 4096MB RAM
- Create a new virtual hard drive, keep the default type (assuming you're staying in VirtualBox)
- Select Dynamically allocated (like thin provisioned in VMware)
- Set the size to 1GB and click create

Now that the VM is created,
- Select the VM and click on the settings button.
- Click on System, then the processor tab and provide 2 processors
- Click the Extended Features check box
- Click on storage
- Click on 'Empty' and click the small CD icon next to the CD/DVD drive pull down
- Select chose a virtual file, navigate and select your ESXi ISO
- Select Network and changed Attached to to Host-only Adapter, click OK
- Click the start button
- Your ESXi install will start (use the right ctrl button to release the mouse from the console)

A few things to note, even though VirtualBox shows the VT-x Option available for the VM, ESXi will complain that it is not available so booting 64-Bit VMs may be a problem (testing to come).  Also don't set the network adapter to bridge or internal only causes PSODs.
