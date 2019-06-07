---
layout: post
author: Jonathan Frappier
title: Getting to know Stacki for Linux installs
permalink: /:title/
modified: 2015-11-27
category: articles
tags: [stacki, linux, automated OS install, bare metal]
share: true
---
Stacki is a project I came across on Twitter that allows you to perform bare metal installations of Linux operating systems, there is a <a href="http://www.stacki.com/intro/" target="_blank">great introduction post on their project page</a> so I am not going to rehash all of that but at a high level:
<ul>
	<li>Install linux for bare metal or virtual machines</li>
	<li>Configure RAID and network controllers</li>
</ul>
The main component of Stacki is the front end which you will install to manage your deployments that deploys "backends" which is simply the term used for the servers that Stacki builds.

A few requirements; Stacki requires that there be no DHCP server on the network, as it allow for PXE booting of the backends. For testing I have attached this only to the management network, which does have DHCP. Obviously in a production environment this could be very bad as machines booting might PXE boot. Additionally, you will need at least 64GB of drive space to complete the install.

The project team claims that "Stacki is the world’s fastest and easiest-to-use Linux server provisioning tool." While I am reasonable good with Linux, let's see how easy it really is! Stacki downloads as an ISO, so boot your virtual machine or host with a the ISO mounted

<img src="/images/fulls/stacki-install.png" class="fit image">

The installation follows a pretty typical linux install wizard, so I won't document that in detail but essential:
<ul>
	<li>Set time zone</li>
	<li>Select the network device (in case of multihomed configurations)</li>
	<li>Provide fqdn, name, IP and DNS information</li>
	<li>Enter a password</li>
	<li>Select disk configuration and which to install (select both)</li>
</ul>
Once deployed, log into the front end as root / &lt;password you set&gt; and run insert-ethers, power on the new machine you want to PXE boot from Stacki and...yup...my backend has PXE booted and started the CentOS 6 installer

<img src="/images/fulls/stacki-backend.png" class="fit image">

And here is the virtual machine with the installation complete, it appears at least as a default that it uses the same root password as the front end appliance.

<img src="/images/fulls/backend-installed.png" class="fit image">

So, it appears the folks at Stacki might be right, that was super easy. Now this is only a default/vanilla install so up next is getting more comfortable with the <a href="https://github.com/StackIQ/stacki/wiki/Concepts" target="_blank">Stacki concept of "pallets" and "wires."</a>