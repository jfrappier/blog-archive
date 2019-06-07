---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide - Prepare Host Clusters for NSX Dejunked
permalink: /:title/
modified: 2016-11-29
category: nsx
tags: [vmware, nsx]
share: true
related: [
    "NSX Installation Guide Dejunked",
    "NSX Installation Guide - Deploy NSX Controller Cluster Dejunked",
    "NSX Installation Guide - Install the NSX Manager Virtual Appliance Dejunked"
]
---
**NSX Installation Guide**

This post covers 3 chapters from the NSX Installation Guide, chapters 10 through 12 and includes preparing a cluster, adding, and removing a host from a prepared cluster. (Original Number of pages 7)

*Prepare Host Clusters for NSX - Chapter 10*
Host preparation is the process in which the NSX kernel modules (VIBs: specifically, esx-vsip and esx-vxlan) are installed on ESXi hosts from NSX Manager (initiated via the vSphere Web Client). The VIBs provide distributed routing, and firewall, and VXLAN bridging. When you prepare a cluster, when a new host is added to the vCenter cluster, the VIBs will automatically be installed on the new host. Note that for stateless ESXi servers you must make the NSX VIBs part of the image. Check [VMware KB 2041972](https://kb.vmware.com/kb/2041972).

Before preparing your hosts/clusters NSX Manager must have been deployed and registered with vCenter, hostnames for NSX Manager and ESXi host must resolve properly (don't forget reverse lookups!), time synchronized properly, and all hosts attached to a common VDS. An interesting pre-req, VMware states that VUM must be disabled before preparing hosts (I wouldn't have guessed that). Finally, verify that there are no issues in the cluster by checking the Actions menu From Networking & Security > Installation > Host Preparation > Actions for a "resolve" option. If one does not exist, you are clear to proceed.

Finally, select the cluster you wish to prepare (Networking & Security > Installation > Host Preparation) and click Actions > Install and wait for the installation to complete before preparing other clusters, or adding additional services. You can verify the installation of the VIBs by running esxcli software vib list | grep esx when SSH'd into a prepared host. A host reboot is not required for install, but is required for removal.

*Add a Host to a Prepared Cluster - Chapter 11*

- Add the host to vCenter as a standalone host
- Connect to the VDS used by NSX
- Place host in maintenance mode
- Add to prepared cluster; this will automatically install the VIBs
- Remove from maintenance mode

*Remove a Host from an NSX Prepared Cluster - Chapter 12*  

- Place host into maintenance mode
- Remove host from prepared cluster
- Reboot

You can verify VIBs were removed by running esxcli software vib list | grep esx. If they were not removed you can run esxcli software vib remove --vibname=esx-vxlan or esx-vsip.


Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf