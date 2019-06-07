---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide - Deploy NSX Controller Cluster Dejunked
permalink: /:title/
modified: 2016-11-28
category: nsx
tags: [vmware, nsx]
share: true
related: [
    "NSX Installation Guide Dejunked",
    "NSX Installation Guide - Preparing for Installation Dejunked",
    "NSX Installation Guide - Install the NSX Manager Virtual Appliance Dejunked"
]
---
**NSX Installation Guide**

So I skipped a couple of chapters; Chapter 6 is for configuring a syslog server, and chapter 7 for adding a license file  - each just a bit over 1 page and pretty spot on already.

*Deploy NSX Controller Cluster - Chapter 8*  (Original Number of pages 3)

NSX Controller is part of the control plane in NSX, and is responsible logical switching, and routing; maintaining  information about the hosts, switches, VXLAN, and DLRs deployed. Currently the only supported configuraiton is 3 NSX Controllers in a controller cluster.

*Prerequisites*
- Datastore (preferably shared) with an average write latency of less than 100ms, and peaks of no more than 300ms.
- NSX Manager deployed and registered with vCenter
- IP Pool created or other means to supply controller IP configuraiton information availble

While not listed, you also need to ensure that there is a L2 subnet the controllers can be deployed on; you should not deploy clusters over a routed network.

To deploy NSX Controllers, navgiate to Networking & Security from the vSphere web client >> Installation >> Management tab
- Click the green plus sign to add a controller
- Select the NSX Manager, data center, cluster, datastore, host, folder, VDS to connect the cluster to and the IP Pool. When selecting the host, select 3 different hosts now so your anti-affinity rule doesnt have any work to do later)
- Provide a password for the NSX Controller (12 characters with 3 of the 4 rules: 1 upper case, 1 lower case, 1 number, 1 special character) (Bonus points - who knows why they are referred to as "upper case" and "lower case" letters?)
- Wait for the controller to finish deploying, and repeat the above steps until 3 controllers are deployed
- Create DRS anti-affinity rules so the controllers are not running on the same host

 Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf