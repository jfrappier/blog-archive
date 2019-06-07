---
layout: post
author: Jonathan Frappier
title: Error during deployment of vCenter Server Appliance or Platform Services Controller following error
permalink: /:title/
modified: 2015-02-10
category: vcsa
tags: [vmware, vsphere, vcenter, vsphere 6, psc]
share: true
---
Scenario: You try to install the VMware vCenter Server Appliance (VCSA) or Platform Services Controller but receive an error during the installation. After correcting the problem during installation you attempt to re-install the appliance but receive the following error message:

<img src="/images/fulls/virtual-machine-already-exists.png" class="fit image">

As of the release candidate of vSphere 6.0, the vCenter Server Appliance installation wizard does not clean up deployed virtual machines after failed deployments. Virtual Machines deployed are still present on the selected ESXi hosts inventory. Log into the ESXi host, power off, and delete the virtual machine from the failed deployment.

<img src="/images/fulls/vcsa-failed-deployment-vms-not-delete.png" class="fit image">

If you attempt to redploy the virtual machine with a different name (appliance and host name) using the same IP address you receive the following error message:

<em><strong>Encountered an internal error. see /var/log/firstboot/vmafd-firstboot.py_6399_stderr.log</strong></em>

Because the virtual machine was deployed and powered on, there is a duplicate IP address on the network.