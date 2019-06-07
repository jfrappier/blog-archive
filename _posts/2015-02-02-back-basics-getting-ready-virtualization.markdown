---
layout: post
author: Jonathan Frappier
title: Back to basics - Getting ready for virtualization
permalink: /:title/
modified: 2015-02-02
category: articles
tags: [vmware, vsphere, vcenter, home lab, back to basics]
share: true
---
Before you get started with virtualization in your environment there are a few things you will need to have in place.

First and foremost you will need a working network in place to provide the various components of you vSphere solution connectivity to one another.  A working DNS solution must be in place, in most cases this is provided by the Domain Controllers in a Windows Active Directory environment.  DNS and AD will support both name resolution of your hosts as well as Single Sign-On (SSO) used by vCenter.

In addition to DNS, NTP is very important, especially if you plan to introduce solutions such as <a title="vSphere SSO or vRA Identity Appliance – vRealize Automation Series Part 1" href="/vsphere-sso-vra-identity-appliance-vrealize-automation-series/">vRealize Automation</a>. Even if virtual machines are just a few minutes off, products may not work properly.

Here are the components in a typical setup:
<ul>
	<li>1 or more switches, preferably with support for VLANs</li>
	<li>Defined IP scheme and IP documentation solution (spreadsheet, IPAM tool etc)</li>
	<li><a title="VMware Workstation Home Lab Setup Part 4 – Domain Controller setup" href="/vmware-workstaion-home-lab-setup-part-4-domain-controller-setup/">LDAP server, typically Microsoft Windows</a></li>
	<li>DNS server, typically provided by Windows Domain Controller</li>
	<li>NTP server, typically running on linux but you could use your DC as well</li>
</ul>
Once the above items are in place, you can now create DNS records for each of your ESXi hosts.  With DNS records created, you can breeze through the installation and configuration of ESXi via the Direct Console User Interface (DCUI). Additionally, kickstart files can be used to configure your ESXi hosts during installation by pressing "shift-O" during startup to access to the boot options or by using Auto Deploy. If you do opt for Auto Deploy, consider a management cluster built on ESXi hosts installed to local disk or SD/USB drives. In this cluster you would run your critical services such as AD, DHCP, DNS, and vCenter so that during an outage you have the ability to recover core services that do not rely on the very services you are trying to restore. Once the management cluster is restored, Auto Deploy can service. <a href="http://professionalvmware.com/2014/04/vbrownbag-follow-up-vmware-vsphere-auto-deploy-deep-dive-with-rob-nelson-rnelson0/" target="_blank">Rob Nelson covers Auto Deploy nicely on a #vBrownBag over at professionalvmware.com</a>.

Other components you need to also consider installing during production builds include a centralized syslog server (vCenter provides one for free, or Log Insight which is an enterprise grade solution. You can also use syslog-ng, Graylog, Splunk, or Nagios Log Monitor. Other tools that ship with vCenter include the dump collector, VMware Update Manager, the VMware Support Assistant and some type of monitoring solution such as Nagios, Realize Operations (vR Ops) or Hyperic. With a working environment ready, you can move on to installing your first ESXi host.

Of course this is a basic list, even the 5 bullet points I listed could consume months of learning if you're not familiar with VLANs, or for planning IP schemas. If you've read though this list and not overwhelmed, you shouldn't have a problem virtualizing if you have not already.