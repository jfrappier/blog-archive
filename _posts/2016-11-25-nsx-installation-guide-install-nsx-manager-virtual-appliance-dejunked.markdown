---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide - Install the NSX Manager Virtual Appliance Dejunked
permalink: /:title/
modified: 2016-11-25
category: nsx
tags: [vmware, nsx]
share: true
related: [
    "NSX Installation Guide Dejunked",
    "NSX Installation Guide - Preparing for Installation Dejunked"
]
---
**NSX Installation Guide**
 
*Install the NSX Manager Virtual Appliance - Chapter 3* (Original Number of pages 5)

NSX manager is deployed as a single virtual machine, available as an OVA from VMware. NSX Manager is responsible for providing the GUI in the vSphere Web Client, for providing REST APIs, and configuration of NSX components. The only HA option for NSX Manger is deploying in a cluster that utilizes vSphere HA. 

**A few important points to remember:**

- Each NSX Manager has a 1 to 1 relationship with vCenter.
- Multiple NSX Managers can be deployed for each vCenter support, however when using enhanced linked mode in vCenter, you can have a maximum of 9 secondary NSX Managers.
- Having multiple NSX Managers does not require enhanced linked mode, however you will need to log into the vSphere Web Client for each vCenter in order to configure and manage NSX.
- Each NSX Manager has a unique UUID.
- Do not upgrade the VMware Tools that comes preinstalled in the VM.

**Preparing for installation:**
- Verify NTP is accessible
- Create forward and reverse DNS records for each NSX Manager
- Ensure that all ESXi hosts, and vCenter Servers have forward and reverse DNS records created
- The computer where you are deploying the OVA from must have the client integration plugin for the vSphere Web Client installed and working

**Installation**
1. Deploy the OVA and follow the wizard
2. Change the name of the VM as required in the wizard
3. VMware recommends changing the disk format to Thick Provision
4. Deploy the the appropriate port group. (Note NSX Manager needs to have access to vCenter, ESXi, and other components as listed the [Preparing for Installation](http://jfrap.com/nsx-installation-guide-preparing-for-installation-dejunked/) section.
5. When the deployment completes, you should be able to log into https://fqdn-of-your-nsx-manager as admin with the password set during installation.

Now that NSX Manager has been deployed, the next steps is to configure and license NSX.
 
 Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf