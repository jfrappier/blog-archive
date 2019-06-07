---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide - Preparing for Installation Dejunked
permalink: /:title/
modified: 2016-11-23
category: nsx
tags: [vmware, nsx]
share: true
related: [
    "NSX Installation Guide Dejunked",
	"NSX Installation Guide - Install the NSX Manager Virtual Appliance Dejunked"
]
---
In part two, I will dive into the Preparing for Installation section. There does appear to be a bit more meat here in the official documentation, but I will do my best to slim it down.

**NSX Installation Guide**
 
*Preparing for Installation - Chapter 2* (Original Number of pages 15)

** System Requirements for NSX**

NSX requires vCenter 5.5U3 or higher, and each component has unique resource requirements. The chart from the install guide shows it best: 
 
<img src="/images/fulls/nsx-ch2-hardware.jpg" class="fit image">
* With more than 256 hypervisors, increase NSX manager to 8 vCPU and 24GB RAM.

For cross vCenter NSX, you need at least NSX Manager/Controller 6.2, vCenter/ESXi 6.0 or later and vCenters must be in enhanced linked mode.

ESXi host names must have both forward and reverse DNS records. NSX Controllers must exist on the same L2 subnet.

The official documentation guide has a long list of required ports, I have attempted to disambiguate that for you here (though use what is best for your learning style)

<img src="/images/fulls/nsx-ch2-ports.jpg" class="fit image">

**NSX and vSphere Distributed Switches**

All hosts that will utilize NSX must be connected to a common VDS, if hosts are only using standard switches, migrate them to a VDS. See the [VMware documentation for your version - e.g. vSphere 6.0](https://pubs.vmware.com/vsphere-60/index.jsp?topic=%2Fcom.vmware.vsphere.networking.doc%2FGUID-D21B3241-0AC9-437C-80B1-0C8043CC1D7D.html)

**NSX Installation Workflow**

The following steps should be followed to deploy NSX successfully.

1. Deploy NSX Manager
2. Register with vCenter 
    2. Configure IP pools or DHCP for NSX components (not an official step on the guide, but important)
3. Deploy NSX Controllers
4. Prepare hosts/clusters
5. Configure networking (logical switches, transport zones, DLR, ESG) 

**Cross vCenter NSX and Enhanced Linked Mode**

Enhanced Linked Mode connects multiple vCenters via one or more PSCs, with this functionality you can also manage all NSX managers from a single web client session. Each vCenter requires an NSX Manager. On the primary NSX Manager, you can create universal components such as switches and routers however Enhanced Linked Mode is NOT required for universal components, but you will not be able to manage other NSX managers from a single web client session.
 
Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf