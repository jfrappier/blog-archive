---
layout: post
author: Jonathan Frappier
title: NSX Installation Guide - Register vCenter Server with NSX Manager & Configure SSO Dejunked
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
 
*Register vCenter Server with NSX Manager - Chapter 4 & Configure Single Sign On - Chapter 5*  (Original Number of pages 5) 

Once NSX has been deployed, and you can access the NSX Manager UI as the default admin user, you will configure settings such as NTP and syslog, and register NSX Manger with vCenter. You may recall from previous posts, there is a 1-to-1 relationship between NSX Manager and vCenter.

The NSX Installation Guide has you first click on Manage vCenter Registration, however I would suggest first clicking on Manage Appliance Settings to confirm network configuration, and NTP information. Now click Manage vCenter Registration:

First, click the Edit button for vCenter Server: 
- Enter the vCenter Server, username, and password. The documentation suggests administrator@vsphere.local, but I would suggest creating a local @vsphere.local user to use for registration in accordance with your company policies
- Accept the certificate

You should see the status return as Connected. If you are logged into the vSphere Web Client, log out and back in as the user who registered vCenter Server and you should see the Networking and Security plugin available from the home screen.

Next, click the edit button for Lookup Service
- Enter the hostname for the lookup service (PSC or vCenter server)
- Enter the port number (443 for vSphere 6 and 7444 for vSphere 5.5)
- Enter a username and password
- Accept the certificate
 
 Reference: https://pubs.vmware.com/NSX-62/topic/com.vmware.ICbase/PDF/nsx_62_install.pdf