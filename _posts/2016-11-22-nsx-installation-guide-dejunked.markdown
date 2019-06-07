---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide Dejunked
permalink: /:title/
modified: 2016-11-22
category: nsx
tags: [vmware, nsx]
share: true
related: [
    "NSX Installation Guide - Preparing for Installation Dejunked",
    "NSX Installation Guide - Install the NSX Manager Virtual Appliance Dejunked",
    "So you want to be a full stack engineer? Advice on mapping out your career."
]
---
Let me start by saying this, I love VMware - the products, many of the people there I consider good friends - BUT when I pick up an install, or administration guide, or some other form of documentation I tune out in about 2 seconds flat when I start reading about the "Software Defined Data Center" journey/platform/foundation/cloud/whatever. What I wanted to try doing is to remove the marketing spin. With that, I present to you my first attempt at "dejunking" documentation and delivering just the core pieces.

**NSX Installation Guide**
 
*Overview of NSX - Chapter 1* (Original Number of pages 7)

NSX is a network virtualization and automation platform that enables the rapid provisioning of L2-L7 network services such as DHCP, load balancing, and firewall services.
 
The components of NSX include:
    - NSX Manager
    - NSX Controller
    - NSX Edge
    - NSX VIBs (kernel modules, user space agents, configuration files, install scripts)
 
NSX breaks these components into 3 planes.
 
**Data Plane**
 
The data plane consists of the kernel modules that provide firewall, Distributed Logical Router (DLR), and VXLAN as well as NSX Edge virtual appliance.

There are two types of NSX Edges; the Edge Services Gateway (ESG) and the DLR.

The ESG provides access to firewall, NAT, DHCP, VPN, and load balancing. You can deploy the ESG in multiple sizes depending on throughput requirements, as well as in a single appliance model, or in HA model which deploys a 2nd ESG. You can configure 10 interfaces per ESG, with a total of 200 possible sub interfaces when connected in a trunk. The uplink, or external interface, connects to a shared or public network to provide connectivity.

The DLR is responsible for east-west routing, enabling VMs in different subnets/portgroups on the same host to communicate without needing to traverse the network. A DLR can have up to 8 uplink interfaces, and 1000 internal interfaces. The **DLR control VM** receives information from NSX Manager and NSX controllers, and exchanges routing information with next hop L3 devices such as an ESG or physical router or L3 switch, and ESXi NSX kernel modules. The **DLR kernel modules** receive routing table information from the NSX controllers, handles route and ARP lookups. Logical Interfaces (LIFs) connect to logical switches. Each LIF has a unique IP address.
 
**Control Plane**
 
NSX Controllers run in the control plane, and is the only component in this layer. The NSX Controller manages switching and routing information that is sent to the data plane. Note that data does not pass through the controllers, and the controllers do not need to be online for the network to function.
 
Controllers, which are deployed by NSX Manager, must be deployed in a 3-node cluster for availability. Controllers have several roles, and these roles are "sliced" such that each role is spread across all controllers. The roles include: 
    - API provider
    - Persistence server
    - Switch manager
    - Logical manager
    - Directory server

NSX supports VLXLAN through 3 switch modes: multicast, unicast, and hybrid. With unicast and hybrid mode, the physical network does not need to support VXLAN.

**Management Plane**

NSX Manager resides in the management plane, providing management and API entry points. There is a 1-to-1 NSX Manager to vCenter server relationship. In a cross-vCenter environment, there is a primary and secondary NSX Manager (maximum of 7 secondary managers). NSX Manager is used for initial configuration and integration with the vSphere Web Client. Once integrated, actions such as logical switch, DLR, or ESG creation can occur. The NSX Manager UI is what you interact with in the vSphere Web Client, even though there is a UI for NSX Manager to perform the initial configuration.

**Consumption Platform**

VMware defines the consumption platform as any interface which can consume NSX resources such as vRealize Automation or the vSphere Web Client.

**Logical Components**

- Logical switches or NSX vSwitch can be distributed across hosts, or vCenters in a cross-vCenter 
- Logical Routers provides east-west and north-south routing capabilities. 
- Logical Firewall allows segmentation based on various attributes such as VM name or user name and can be applied to data centers, hosts, or virtual machines. ESGs also provide firewall services in a more traditional manner; that is VMs behind the ESG will have traffic filtered by the firewall rules configured on the specific ESG.
- Logical load balancer is a function of the ESG and provides standard load balancing options. Load balancing can be performed in either one arm mode, or more traditional manner with virtual machines behind a NAT.
- Service Composer allows you to create security services such as a firewall rule that only allows port 80 or 443 to a virtual machine, and multiple services can be added to a security group. Security groups can have dynamic or static membership, and can be consumed through CMPs such as vRA.
 
 Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf