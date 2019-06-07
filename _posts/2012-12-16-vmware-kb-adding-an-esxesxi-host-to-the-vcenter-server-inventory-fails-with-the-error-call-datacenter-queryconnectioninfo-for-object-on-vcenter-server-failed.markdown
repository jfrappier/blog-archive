---
layout: post
author: Jonathan Frappier
title: VMware KB Adding an ESX/ESXi host to the vCenter Server inventory fails with the error Call datacenter.queryconnectioninfo for object on vCenter Server failed
permalink: /:title/
modified: 2012-12-16
category: articles
tags: [kb, vmware, management, agent, vcenter server inventory, vsphere 5]
share: true
---
I ran into this problem while setting up my lab.  VMware has KB (link below) on this article but wasn't much help for me, though I always start with the KB.  The fix for me (I had two servers doing this) was to either restart the management agent via the DCUI and then add, or add the host via IP address then remove it from vCenter and re-add.  These may not work for you, but since the KB article didn't work for me, maybe this will help.

<a href="http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&amp;cmd=displayKC&amp;externalId=1027672#.UM22EocQZ1Y.wordpress">VMware KB: Adding an ESX/ESXi host to the vCenter Server inventory fails with the error: Call "datacenter.queryconnectioninfo" for object on vCenter Server failed</a>.